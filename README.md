// 分类筛选模块 - TMDB 电影分类筛选
WidgetMetadata = {
  id: "tmdb.movie.category",
  title: "TMDB 电影分类",
  version: "1.0.0",
  requiredVersion: "0.0.1",
  description: "TMDB 电影分类筛选模块，支持按类型、语言、日期筛选",
  author: "C",
  site: "https://www.themoviedb.org",
  icon: "https://www.themoviedb.org/assets/2/favicon-32x32-543a21832c8931d3494a68881f6afcafc58e96c5d324345377f3197a37b367b5.png",
  detailCacheDuration: 3600,
  globalParams: [],
  modules: [
    {
      id: "movieByCategory",
      title: "电影分类",
      functionName: "loadMoviesByCategory",
      cacheDuration: 1800,
      requiresWebView: false,
      sectionMode: false,
      params: [
        {
          name: "genre",
          title: "类型",
          type: "enumeration",
          value: "28",
          enumOptions: [
            { title: "动作", value: "28" },
            { title: "冒险", value: "12" },
            { title: "动画", value: "16" },
            { title: "喜剧", value: "35" },
            { title: "犯罪", value: "80" },
            { title: "纪录片", value: "99" },
            { title: "剧情", value: "18" },
            { title: "家庭", value: "10751" },
            { title: "奇幻", value: "14" },
            { title: "恐怖", value: "27" },
             { title: "音乐", value: "10402" },
            { title: "悬疑", value: "9648" },
            { title: "爱情", value: "10749" },
            { title: "科幻", value: "878" },
            { title: "电视电影", value: "10770" },
            { title: "惊悚", value: "53" },
            { title: "战争", value: "10752" },
            { title: "西部", value: "37" }
          ]
        },
        {
          name: "language",
          title: "语言",
          type: "language",
          value: "zh-CN"
        },
        {
          name: "releaseDate",
          title: "发布日期",
          type: "input",
          placeholders: [{ title: "YYYY-MM-DD (例如: 2024-01-01)", value: "" }]
        },
        {
          name: "sortBy",
          title: "排序方式",
          type: "enumeration",
          value: "popularity.desc",
          enumOptions: [
            { title: "热门度降序", value: "popularity.desc" },
            { title: "热门度升序", value: "popularity.
            asc" },

{ title: "评分降序", value: "vote_average.desc" },

{ title: "评分升序", value: "vote_average.asc" },

{ title: "最新发布", value: "primary_release_date.desc" },

{ title: "最早发布", value: "primary_release_date.asc" },

{ title: "票房降序", value: "revenue.desc" },

{ title: "票房升序", value: "revenue.asc" }

]

},

{

name: "page",

title: "页码",

type: "page"

}

]

}

],

search: {

title: "搜索电影",

functionName: "searchMovies",

params: [

{ name: "keyword", title: "关键词", type: "input" },

{ name: "page", title: "页码", type: "page" }

]

}

};

// TMDB 分类电影列表

async function loadMoviesByCategory(params = {}) {

try {

const genreId = params.genre ||
"28"; // 默认动作类型

const language = params.language || "zh-CN";

const releaseDate = params.releaseDate || "";

const sortBy = params.sortBy || "popularity.desc";

const page = Number(params.page || 1);

纯文本
// 构建查询参数
const queryParams = {
  language: language.replace("-", "_"),
  sort_by: sortBy,
  page: page,
  with_genres: genreId
};

// 如果指定了发布日期，添加日期筛选
if (releaseDate && releaseDate.match(/^\d{4}-\d{2}-\d{2}$/)) {
  queryParams["primary_release_date.gte"] = releaseDate;
}

console.log(`[loadMoviesByCategory] 请求参数:`, { genreId, language, releaseDate, sortBy, page });

const res = await Widget.tmdb.get("discover/movie", { params: queryParams });

if (!res || !res.results) {
  throw new Error("TMDB API 返回空响应");
}

纯文本
// 获取类型名称映射
const genreRes = await Widget.tmdb.get("genre/movie/list", { params: { language: language.replace("-", "_") } });
const genreMap = {};
if (genreRes && genreRes.genres) {
  genreRes.genres.forEach(g => {
    genreMap[g.id] = g.name;
  });
}

// 转换结果为 VideoItem 格式
return res.results.map(movie => {
  // 获取当前类型的名称
  const movieGenres = movie.genre_ids ? movie.genre_ids.map(id => ({
    id: String(id),
    title: genreMap[id] || `类型${id}`
  })) : [];
  
  // 获取选中的类型名称
  const selectedGenreName = genreMap[genreId] || `类型${genreId}`;
  
  return {
    id: movie.id,
    type: "tmdb",
    mediaType: "movie",
    title: movie.title || movie.original_title || "未知电影",
    posterPath:
movie.poster_path,

backdropPath: movie.backdrop_path,

rating: movie.vote_average,

releaseDate: movie.release_date,

description: movie.overview || "",

genreItems: movieGenres,

// 添加额外的信息标签

durationText: ${selectedGenreName} | 评分: ${movie.vote_average.toFixed(1)}/10,

// 注意：不要添加 link 字段，因为这是 tmdb 类型，使用内置详情页

};

});

} catch (error) {

console.error("[loadMoviesByCategory] 失败:", error.message || error);

throw new Error(获取电影列表失败: ${error.message});

}

}

// 搜索电影函数

async function searchMovies(params = {}) {

try {

const keyword = params.keyword;

const page = Number(params.page || 1);

纯文本
if (!keyword || keyword.trim() === "") {
  return [];
}

console.log(`[searchMovies] 搜索关键词:
"keyword",页码:{page}`);

纯文本
const res = await Widget.tmdb.get("search/movie", { 
  params: { 
    query: keyword,
    page: page,
    language: "zh-CN"
  }
});

if (!res || !res.results) {
  throw new Error("搜索API返回空响应");
}

return res.results.map(movie => ({
  id: movie.id,
  type: "tmdb",
  mediaType: "movie",
  title: movie.title || movie.original_title || "未知电影",
  posterPath: movie.poster_path,
  backdropPath: movie.backdrop_path,
  rating: movie.vote_average,
  releaseDate: movie.release_date,
  description: movie.overview || "",
  durationText: `评分: ${movie.vote_average.toFixed(1)}/10`,
}));
} catch (error) {

console.error("[searchMovies] 失败:", error.message || error);

throw new Error(搜索电影失败: ${error.message});

}

}

// 加载详情函数（仅用于自定义链接）

async function loadDetail(link) {

// 这个模块使用 TMDB 内置详情页，所以不需要自定义 loadDetail

// 但如果需要添加自定义详情页，可以在这里实现

console.log([loadDetail] 收到链接: ${link});

return null;

}

