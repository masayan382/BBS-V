<template>
	<div class="postarea">
		<h2 class="align-items-center">掲示板に投稿する</h2>
		<div class="mt-4 pt-4 mb-4 pb-4">
			<div class="row align-items-center">
				<div class="col-md-5">ニックネーム</div>
				<div class="col-md-7">
					<input type="text" id="name" v-model="name" />
				</div>
			</div>

			<div class="row align-items-center">
				<div class="col-md-5">コメント</div>
				<div class="col-md-7">
					<textarea id="comment" v-model="comment"></textarea>
				</div>
			</div>

			<div class="col-md-12 mt-4">
				<button class="btn btn-primary pl-4 pr-4" @click="createComment">
					掲示板に送信
				</button>
			</div>
			<hr />
		</div>

		<h2 class="bbs mb-4">掲示板</h2>
		<div v-for="post in posts" :key="post.name" class="mt-2 mb-2">
			<div>名前：{{ post.fields.name.stringValue }}</div>
			<div class="mt-4">コメント：{{ post.fields.comment.stringValue }}</div>
			<hr />
		</div>
	</div>
</template>

<script>
	import axios from 'axios';
	export default {
		data() {
			return {
				name: '',
				comment: '',
				posts: [],
			};
		},
		computed: {
			idToken() {
				return this.$store.getters.idToken;
			},
		},
		created() {
			axios
				.get('/comments', {
					headers: {
						Authorization: `Bearer ${this.idToken}`,
					},
				})
				.then((response) => {
					this.posts = response.data.documents;
					console.log(response.data.documents);
				});
		},
		methods: {
			createComment() {
				axios
					.post(
						'/comments',
						{
							fields: {
								name: {
									stringValue: this.name,
								},
								comment: {
									stringValue: this.comment,
								},
							},
						},
						{
							headers: {
								Authorization: `Bearer ${this.idToken}`,
							},
						}
					)
					.then(() => {
						axios
							.get('/comments', {
								headers: {
									Authorization: `Bearer ${this.idToken}`,
								},
							})
							.then((response) => {
								this.posts = response.data.documents;
								console.log(response.data.documents);
							});
					});
				this.name = '';
				this.comment = '';
			},
		},
	};
</script>

<style scoped>
	.postarea {
		margin-top: 20px;
		display: flex;
		flex-flow: column nowrap;
		justify-content: center;
		align-items: center;
	}
	.container {
		width: 500px;
		border: 3px solid #ccc;
	}
	input {
		margin: 10px 0;
		width: 300px;
		padding: 10px;
		border: 3px solid #ccc;
	}
	textarea {
		border: 3px solid #ccc;
		resize: none;
		width: 300px;
		height: 200px;
	}
	.bbs {
		color: #fff;
		background-color: #ccc;
		width: 0 100%;
		padding: 10px;
	}
</style>
