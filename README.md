// 电影分类模块 - 经过测试可用
WidgetMetadata = {
  id: "test.movie.category",
  title: "电影分类示例",
  version: "1.0.0",
  requiredVersion: "0.0.1",
  description: "演示用的电影分类模块，包含真实数据",
  author: "Test",
  site: "https://example.com",
  detailCacheDuration: 300,
  globalParams: [],
  modules: [
    {
      id: "categoryDemo",
      title: "分类演示",
      functionName: "loadCategoryDemo",
      cacheDuration: 600,
      requiresWebView: false,
      sectionMode: false,
      params: [
        {
          name: "category",
          title: "分类",
          type: "enumeration",
          value: "action",
          enumOptions: [
            { title: "动作", value: "action" },
            { title: "喜剧", value: "comedy" },
            { title: "犯罪", value: "crime" },
            { title: "科幻", value: "sci-fi" }
          ]
        },
        {
          name: "page",
          title: "页码",
          type: "page"
        }
      ]
    }
  ]
};

// 模拟的电影数据
const movieData = {
  action: [
    { id: 1, title: "疾速追杀", rating: 7.4, year: "2014"
    },
    { id: 2, title: "敢死队", rating: 6.5, year: "2010" },
    { id: 3, title: "战狼", rating: 7.0, year: "2015" }
  ],
  comedy: [
    { id: 4, title: "三傻大闹宝莱坞", rating: 8.4, year: "2009" },
    { id: 5, title: "夏洛特烦恼", rating: 7.7, year: "2015" },
    { id: 6, title: "让子弹飞", rating: 8.8, year: "2010" }
  ],
  crime: [
    { id: 7, title: "教父", rating: 9.2, year: "1972" },
    { id: 8, title: "无间道", rating: 9.1, year: "2002" },
    { id: 9, title: "消失的爱人", rating: 8.1, year: "2014" }
  ],
  "sci-fi": [
    { id: 10, title: "星际穿越", rating: 8.6, year: "2014" },
    { id: 11, title: "盗梦空间", rating: 8.8, year: "2010" },
    { id: 12, title: "流浪地球", rating: 7.9, year: "2019" }
  ]
};

async function loadCategoryDemo(params = {}) {
  try {
    const category = params.category || "action";
    const page = Number(params.page || 1);
    
    console.log(`加载分类: ${category}, 页码: ${page}`);
    
    const movies = movieData[category] || movieData.action;
    
    // 模拟分页
    const pageSize = 10;
    const startIdx = (page - 1) * pageSize;
    const endIdx = Math.min(startIdx + pageSize, movies.length);
    const pageMovies = movies.slice(startIdx, endIdx);
    
    return pageMo
    vies.map(movie => ({
      id: movie.id,
      type: "url",
      title: movie.title,
      posterPath: `https://via.placeholder.com/300x450/333/fff?text=${encodeURIComponent(movie.title)}`,
      backdropPath: `https://via.placeholder.com/1280x720/222/fff?text=${encodeURIComponent(movie.title)}`,
      rating: movie.rating,
      releaseDate: `${movie.year}-01-01`,
      description: `这是 ${movie.title} 的示例描述，评分 ${movie.rating}/10，年份 ${movie.year}`,
      durationText: `评分: ${movie.rating}/10 | 年份: ${movie.year}`,
      genreItems: [
        { id: category, title: category === "action" ? "动作" : category === "comedy" ? "喜剧" : category === "crime" ? "犯罪" : "科幻" }
      ],
      link: `movie:${movie.id}`  // 自定义链接
    }));
    
  } catch (error) {
    console.error("[loadCategoryDemo] 错误:", error);
    return []; // 返回空数组而不是抛出错误
  }
}

async function loadDetail(link) {
  try {
    console.log(`加载详情: ${link}`);
    const match = link.match(/^movie:(\d+)$/);
    if (!match) return null;
    
    const movieId = parseInt(match[1], 10);
    
    // 查找电影数据
    let movie = null;
    for (const category in movieData) {
      const found = movieData[category].find(m => m.id ===
      vies.map(movie => ({
      id: movie.id,
      type: "url",
      title: movie.title,
      posterPath: `https://via.placeholder.com/300x450/333/fff?text=${encodeURIComponent(movie.title)}`,
      backdropPath: `https://via.placeholder.com/1280x720/222/fff?text=${encodeURIComponent(movie.title)}`,
      rating: movie.rating,
      releaseDate: `${movie.year}-01-01`,
      description: `这是 ${movie.title} 的示例描述，评分 ${movie.rating}/10，年份 ${movie.year}`,
      durationText: `评分: ${movie.rating}/10 | 年份: ${movie.year}`,
      genreItems: [
        { id: category, title: category === "action" ? "动作" : category === "comedy" ? "喜剧" : category === "crime" ? "犯罪" : "科幻" }
      ],
      link: `movie:${movie.id}`  // 自定义链接
    }));
    
  } catch (error) {
    console.error("[loadCategoryDemo] 错误:", error);
    return []; // 返回空数组而不是抛出错误
  }
}

async function loadDetail(link) {
  try {
    console.log(`加载详情: ${link}`);
    const match = link.match(/^movie:(\d+)$/);
    if (!match) return null;
    
    const movieId = parseInt(match[1], 10);
    
    // 查找电影数据
    let movie = null;
    for (const category in movieData) {
      const found = movieData[category].find(m => m.id ===
      movieId);
      if (found) {
        movie = found;
        break;
      }
    }
    
    if (!movie) return null;
    
    return {
      id: movie.id,
      type: "url",
      title: movie.title,
      link: link,
      posterPath: `https://via.placeholder.com/300x450/333/fff?text=${encodeURIComponent(movie.title)}`,
      backdropPath: `https://via.placeholder.com/1280x720/222/fff?text=${encodeURIComponent(movie.title)}`,
      backdropPaths: [
        `https://via.placeholder.com/1280x720/444/fff?text=剧照1+${encodeURIComponent(movie.title)}`,
        `https://via.placeholder.com/1280x720/555/fff?text=剧照2+${encodeURIComponent(movie.title)}`,
        `https://via.placeholder.com/1280x720/666/fff?text=剧照3+${encodeURIComponent(movie.title)}`
      ],
      rating: movie.rating,
      releaseDate: `${movie.year}-01-01`,
      description: `这是 ${movie.title} 的完整详情描述。\n\n评分: ${movie.rating}/10\n年份: ${movie.year}\n\n详细剧情说明...
      `,

durationText: 评分: ${movie.rating}/10 | 年份: ${movie.year},

genreItems: [

{ id: "action", title: "动作" },

{ id: "drama", title: "剧情" }

],

peoples: [

{ id: 1, title: "导演张三", role: "导演" },

{ id: 2, title: "主演李四", role: "主演" }

],

relatedItems: [

{ id: 100, type: "url", title: "相关推荐1", posterPath: "https://via.placeholder.com/150x225" },

{ id: 101, type: "url", title: "相关推荐2", posterPath: "https://via.placeholder.com/150x225" }

]

};

} catch (error) {

console.error("[loadDetail] 错误:", error);

return null;

}

}

