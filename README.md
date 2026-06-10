// 分类筛选模块 - TMDB 电影分类筛选
WidgetMetadata = {
  id: "tmdb.movie.category",
  title: "TMDB 电影分类",
  version: "1.0.0",
  requiredVersion: "0.0.1",
  description: "TMDB 电影分类筛选模块",
  author: "C",
  site: "https://www.themoviedb.org",
  icon: "https://www.themoviedb.org/assets/2/favicon-32x32.png",
  detailCacheDuration: 3600,
  globalParams: [],
  modules: [{
    id: "movieByCategory",
    title: "电影分类",
    functionName: "loadMoviesByCategory",
    cacheDuration: 1800,
    params: [
      { name: "genre", title: "类型", type: "enumeration", value: "28", enumOptions: [
        { title: "动作", value: "28" }, { title: "喜剧", value: "35" }, { title: "犯罪", value: "80" }
      ]},
      { name: "language", title: "语言", type: "language", value: "zh-CN" },
      { name: "releaseDate", title: "发布日期", type: "input" },
      { name: "page", title: "页码", type: "page" }
    ]
  }]
};

async function loadMoviesByCategory(params = {}) {
  try {
    // 简化的演示数据
    return [
    {
        id: 1, type: "tmdb", mediaType: "movie",
        title: "示例电影1", posterPath: "/abc.jpg",
        rating: 8.5, releaseDate: "2024-01-01",
        description: "示例电影描述",
        durationText: "评分: 8.5/10"
      },
      {
        id: 2, type: "tmdb", mediaType: "movie",
        title: "示例电影2", posterPath: "/def.jpg",
        rating: 7.8, releaseDate: "2024-02-01",
        description: "另一个示例",
        durationText: "评分: 7.8/10"
      }
    ];
  } catch (error) {
    console.error("错误:", error);
    throw new Error("加载失败");
  }
}
