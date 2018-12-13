<template>
	<div>
		<div class="form-group" align="right">
		  <input class="form-control" style="width: 30%"   placeholder="Search Article" v-model="keywords" v-on:keyup.enter = "searchArticle()" />
    	 </div>

		<h3> Add New Article</h3>
		
		<form @submit.prevent = "addArticle()" class="mb-3">
		<div class="form-group">
		<input class="form-control"  v-model="article.title" placeholder="Enter the title....">
		</div>
		<div class="form-group">
		<textarea class="form-control" v-model="article.body" placeholder="Enter the body..."></textarea></div>
		<div class="form-group " >
		<button type="submit" style="width: 20%" class="btn btn-success btn-block">Save</button>
		<button @click="clearForm()" style="width: 20%"  class="btn btn-warning btn-block">Cancel</button></div>
		</form>
		<nav aria-label="Page navigation example">
        <ul class="pagination">
	    <li v-bind:class = "[{disabled: !pagination.prev_page_url}]" class="page-item"><a class="page-link" href="#" @click="fetchArticles(pagination.prev_page_url)">Privious</a></li>

	    <li class="page-item disabled"><a class="page-link text-dark" href="#">Page {{ pagination.current_page }} of {{ pagination.last_page }} </a></li>

	    <li v-bind:class = "[{disabled: !pagination.next_page_url}]" class="page-item"><a class="page-link" href="#" @click="fetchArticles(pagination.next_page_url)">Next</a></li>

	    </ul></nav>
		<div class="card card-body mb-2" v-for="article in articles" v-bind:key = "article.id"  >
			<h3>{{ article.title }}</h3>
			<p>{{ article.body }}</p>
			<div>	
			<button class="btn btn-danger" @click="deleteArticle(article.id)">Delete</button>
			<button class="btn btn-info" @click="editArticle(article)">Edit</button></div>
		</div>
	</div>
</template>
<script>
        export default {
    data(){
    return {
        articles: [],
        article: {
          id: '',
        title: '',
        body: '',
        },
        keywords: '',
        page_no: '',
        article_id: '',
        pagination: {},
        edit: false
        }
        },
    created(){
        this.fetchArticles();
        },

        methods:
        {
        fetchArticles(page_url){
        let vm = this;
	    page_url = page_url || '/api/article';
        fetch(page_url).then(res => res.json()).then(res =>{
          this.articles = res.data;
          vm.makePagination(res.meta, res.links);})
			.catch(err => console.log(err));
		},
	    makePagination(meta, links) {
	    this.page_no = meta.current_page;
		let pagination = {
			current_page: meta.current_page,
			last_page: meta.last_page,
			next_page_url:links.next,
			prev_page_url:links.prev
		};
	    this.pagination = pagination;
	     },

	     deleteArticle(id){
	       if(confirm('Are you sure?')){
	       fetch(`api/article/${id}`,{
	       method: 'delete'
	       })
	       .then(res => res.json())
	       .then(data => {
	       this.fetchArticles('api/article?page='+this.page_no);
	       });
	       }
	     },

	     addArticle(){
	     	if(this.edit === false){
	     	  fetch('api/article',{
	     	  	method: 'post',
	     	  	body: JSON.stringify(this.article),
	     	  	headers: {
	     	  	'content-type':'application/json'
	     	  		}
	     	  })
	     	  .then(res => res.json())
	     	  .then(data => {
	     	  	this.article.title = '';
	     	  	this.article.body = '';
	     	  	this.fetchArticles();
	     	  })
	     	}else{
	     		 fetch('api/article',{
	     	  	method: 'put',
	     	  	body: JSON.stringify(this.article),
	     	  	headers: {
	     	  	'content-type':'application/json'
	     	  		}
	     	  })
	     	  .then(res => res.json())
	     	  .then(data => {
	     	  	this.article.title = '';
	     	  	this.article.body = '';
	     	  	this.fetchArticles('api/article?page='+this.page_no);
	     	  })
	     	}
	     	
	     },

	     editArticle(article){
	     	this.edit = true;
	     	this.article.id = article.id;
	     	this.article.article_id = article.id;
	     	this.article.title = article.title;
	     	this.article.body = article.body;
	     },

	     clearForm(){
	     		this.edit = false;
	     		this.article.id = '';
	     		this.article.article_id = '';
	     	    this.article.title = '';
	     	  	this.article.body = '';
	     
	     	  	},

	     	  	searchArticle(page_url){
	     	  			let sm = this;
	     	  			page_url = 'api/article/'+this.keywords;
	     	  			console.log(page_url);
	     	  			fetch(page_url).then(res => res.json()).then(res =>{
			            this.articles = res.data;
			            sm.makePagination(res.meta, res.links);})
						.catch(err => console.log(err));
					}
				    
				     	  		
				     	  	}
			           
			          
		     };
	  </script>