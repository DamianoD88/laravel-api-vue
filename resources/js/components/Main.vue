<template>

   <main>
       <!-- <div class="container">
           <p> Current Page {{ currentPage }} - Last Page {{ lastPage }} </p>
       </div> -->


        <div class="container">        
            <div class="row">
                <div class="col-sm-6" v-for="post in posts" :key="post.id">
                    <div class="card mt-3">
                        <div class="card-body">
                            <h5 class="card-title">{{ post.title }}</h5>
                            <p>{{ formatData(post.created_at) }}</p>
                            <p class="card-text">{{ truncate(post.content,150) }}</p>
                            <a href="#" class="btn btn-primary">Dettagli</a>
                        </div>
                    </div>
                </div>
            </div>
        </div>
        
        <!-- <div class="container mt-5">
            <nav aria-label="Page navigation example">
                <ul class="pagination">
                    <li class="page-item" 
                    :class="{'disabled' : currentPage == 1}">
                        <button class="page-link"
                         href="#" @click="getPosts(currentPage - 1)">Indietro</button>
                        </li>
                    <li class="page-item"
                    :class="{'disabled' : currentPage == lastPage}">
                        <button class="page-link" href="#" @click="getPosts(currentPage + 1)">Prossima</button>
                        </li>
                </ul>
            </nav>
        </div> -->

        <div class="container mt-3">
            <nav aria-label="Page navigation example">
                <ul class="pagination">
                    <li class="page-item" 
                    :class="{'disabled' : currentPage == 1}">
                        <button class="page-link"
                         href="#" @click="getPosts(currentPage - 1)">Indietro
                         </button>
                    </li>
                    
                    <li class="page-item"
                        :class="{'active': currentPage == i}" 
                        v-for="i in lastPage" :key="i"
                        @click="getPosts(i)">
                        <a class="page-link" href="#">{{ i }}</a>
                    </li>

                    <li class="page-item"
                    :class="{'disabled' : currentPage == lastPage}">
                        <button class="page-link" href="#" @click="getPosts(currentPage + 1)">Prossima</button>
                        </li>
                </ul>
            </nav>

        </div>

   </main>

</template>

<script>

export default {
    name: "Main",
    data() {
        return {
            chiamataApi: 'http://localhost:8000/api/posts',
            posts: [],
            currentPage: 1,
            lastPage: null
        }
    },
    created(){
        this.getPosts(1);
    },
    methods: {
        getPosts(pagePost){
            axios.get(this.chiamataApi, {
                params: {
                    page: pagePost
                }
            })
                .then(response => {
                    
                    this.posts = response.data.results.data;
                    this.currentPage = response.data.results.current_page;
                    this.lastPage = response.data.results.last_page;
                    console.log(this.posts);
                    console.log(response.data.results.current_page);
                    console.log(response.data.results.last_page);
                })
                .catch();
        },
        truncate(text,maxlength){
            if(text.length > maxlength) {
               return text.substr(0, maxlength) + '...';
            }
            return text;
        },
        formatData(data){
            //creazione istanza oggetto date 
            const postData = new Date(data);
            //data completa
            let day = postData.getDate();
            let month = parseInt(postData.getMonth() + 1);
            if(day < 10){
                day = '0' + day; 
            }
            if(month < 10){
                month = '0' + month;
            }
            // restituzione data
            return day + '-' + month + '-' + postData.getFullYear();
        }
    }
}
</script>

<style lang="scss" scoped>

</style>