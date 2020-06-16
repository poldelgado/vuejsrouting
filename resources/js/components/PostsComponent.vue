<template>
    <div class="container">
        <div class="row justify-content-center">
            <div class="col-md-8">
                <div class="card mb-3 mt-3" v-for="item in list">
                    <div class="card-header" :href="item.slug" v-text="item.title"></div>

                    <div class="card-body">
                        <p>ID: {{item.id}}</p>
                        <p class="card-text" v-text="item.body"></p>
                    </div>
                </div>

                <infinite-loading @infinite="infiniteHandler">
                    <div slot="no-more">--</div>
                    <div slot="spinner">Cargando...</div>
                    <div slot="no-result">Sin resultados</div>

                </infinite-loading>

            </div>
        </div>
    </div>
</template>

<script>
    export default {
        mounted() {
            console.log("component montado");
        },
        data() {
            return {
                list: [],
                page: 0
            };
        },
        methods: {
            infiniteHandler($state) {
                this.page++;
                let url = '/api/posts?page='+ this.page;
                //conectar al servidor

                axios.get(url).then( response => {
                    let posts = response.data.data

                    if (posts.length) {
                        this.list = this.list.concat(posts)
                        $state.loaded()
                    } else {
                        $state.complete()
                    }
                });
            },
        },
    }
</script>
