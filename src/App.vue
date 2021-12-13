<template>
    <div id="app"></div>
</template>

<script>
export default {
    name: "App",
    data() {
        return {
            base_image_path: "https://image.tmdb.org/t/p/w500",
            movie_discovery_path:
                "https://api.themoviedb.org/3/discover/movie?api_key=c4ab30e3d2e7ebc7dc5c4d34d7530491&language=en-US&sort_by=popularity.desc&include_adult=false&include_video=false&page=1&with_watch_monetization_types=flatrate",
            movies: [], //[{title, image, overview, date}],
            genres: [
                { id: 28, name: "Action" },
                { id: 12, name: "Adventure" },
                { id: 16, name: "Animation" },
                { id: 35, name: "Comedy" },
                { id: 80, name: "Crime" },
                { id: 99, name: "Documentary" },
                { id: 18, name: "Drama" },
                { id: 10751, name: "Family" },
                { id: 14, name: "Fantasy" },
                { id: 36, name: "History" },
                { id: 27, name: "Horror" },
                { id: 10402, name: "Music" },
                { id: 9648, name: "Mystery" },
                { id: 10749, name: "Romance" },
                { id: 878, name: "Science Fiction" },
                { id: 10770, name: "TV Movie" },
                { id: 53, name: "Thriller" },
                { id: 10752, name: "War" },
                { id: 37, name: "Western" },
            ],
        };
    },

    methods: {
        fetchMovies() {
            fetch(this.movie_discovery_path)
                .then((res) => {
                    let json = res.json();
                    return json;
                })
                .then((movies) => {
                    console.log(movies);

                    var movieBuilder = movies.results.map((movie) => {
                        let genreName = this.getMovieGenreName(movie);

                        return {
                            title: movie.title,
                            release_date: movie.release_date,
                            image: `${this.base_image_path}${movie.poster_path}`,
                            overview: movie.overview,
                            genre: genreName,
                        };
                    });

                    console.log(movieBuilder);
                    return movieBuilder;
                })
                .catch((err) => {
                    console.log(err);
                    alert(err);
                });
        },

        getMovieGenreName(movie) {
            let genre = this.genres.find((i) => i.id === movie.genre_ids[0]);
            console.log(genre.name);
            return genre.name;
        },
    },
    //when vue is mounted on dom
    mounted() {
        //fires on next dom update
        this.$nextTick(function () {
            this.fetchMovies();
        });
    },
};
</script>
