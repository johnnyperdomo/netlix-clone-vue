<template>
    <div id="app" class="main-content vh-100">
        <nav class="navbar navbar-light border-bottom border-4 border-danger">
            <div class="container">
                <a class="navbar-brand" href="#">
                    <img src="./assets/netflix-logo.png" alt="" height="24" />
                </a>
            </div>
        </nav>

        <div id="dyamic-data" v-if="movies.length != 0" class="container mt-8 vh-100">
            <!-- hero -->

            <div id="hero" class="row mt-5">
                <!-- left -->

                <div class="col-12 col-sm-6">
                    <h4 class="text-muted">New Releases</h4>

                    <!-- title -->
                    <h1 class="text-white">{{ movies[1].movies[3].title }}</h1>

                    <!-- overview -->
                    <p class="col-10 text-white mt-4">{{ movies[1].movies[3].overview }}</p>

                    <!-- action buttons -->

                    <div class="btn-toolbar mt-4">
                        <button class="btn btn-light">Play Movie</button>
                    </div>
                </div>

                <!-- right -->

                <div class="col-12 col-sm-6">
                    <div class="col-2 mx-auto">
                        <img
                            v-bind:src="movies[1].movies[3].image"
                            alt=""
                            class="rounded shadow"
                            height="300"
                        />
                    </div>
                </div>
            </div>

            <!-- movie slider components -->

            <div class="mt-5">
                <!-- loop://TODO -->
                <div id="movie-rows" v-for="(movie, i) in movies" v-bind:key="i" class="bg-black">
                    <h3 class="text-white">{{ movie.genre }}</h3>

                    <!-- horizontal slider -->
                    <div class="row mt-3 flex-row flex-nowrap scrollable-wrapper">
                        <!-- card -->

                        <div
                            class="card col-6 col-sm-5 col-md-4 col-lg-3 col-xl-2 mx-2 h-25"
                            style="padding: 0px; border: 0; background-color: black"
                            v-for="(nestedMovie, idx) in movie.movies"
                            v-bind:key="(nestedMovie, idx)"
                        >
                            <img
                                v-bind:src="nestedMovie.image"
                                alt=""
                                class="card-img-top rounded shadow h-100"
                            />

                            <div class="card-body">
                                <h6 class="text-white small">{{ nestedMovie.title }}</h6>
                            </div>
                            <!-- poster image -->
                            <!-- {{ nestedMovie }} -->
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: "App",
    data() {
        return {
            base_image_path: "https://image.tmdb.org/t/p/w500",
            movie_discovery_path:
                "https://api.themoviedb.org/3/discover/movie?api_key=c4ab30e3d2e7ebc7dc5c4d34d7530491&language=en-US&sort_by=popularity.desc&include_adult=false&include_video=false&page=1&with_watch_monetization_types=flatrate",
            movies: [], //[{title, image, overview, release_date, genre}],
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
            examples: [
                { id: 37, name: "Western" },
                { id: 37, name: "Thriller" },
                { id: 37, name: "War" },
                { id: 37, name: "Romance" },
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

                    this.movies = this.getAllGenresInSetThenOrganize(movieBuilder);

                    console.log(this.movies);
                    return this.movies;
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

        getAllGenresInSetThenOrganize(movies) {
            const uniqueGenres = [...new Set(movies.map((item) => item.genre))]; // [ 'A', 'B']

            var newMovieSet = [];

            uniqueGenres.map((genre) => {
                let filter = movies.filter((movie) => movie.genre === genre);

                newMovieSet.push({ genre: genre, movies: filter });
            });

            return newMovieSet;
        },
    },
    //when vue is mounted on dom
    mounted() {
        //fires on next dom update
        this.$nextTick(function () {
            this.fetchMovies(); //TODO
        });
    },
};
</script>

<style scoped>
#app {
    background: black;
}

.scrollable-wrapper {
    overflow-x: auto;
}

.body {
    background: black;
}
</style>
