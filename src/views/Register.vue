<template>
	<div class="register">
		<h2>登録</h2>
		<div class="container mt-4 pt-4 mb-4 pb-4">
			<div class="row align-items-center">
				<div class="col-md-4">Email</div>
				<div class="col-md-3">
					<input
						type="email"
						id="email"
						v-model="email"
						placeholder="emailアドレス"
					/>
				</div>
			</div>

			<div class="row align-items-center">
				<div class="col-md-4">パスワード</div>
				<div class="col-md-3">
					<input
						type="password"
						id="password"
						v-model="password"
						placeholder="半角英数6文字以上"
					/>
				</div>
			</div>
			<div class="row align-items-center">
				<div class="col-md-12 mt-4">
					<button class="btn btn-primary" @click="register">登録</button>
				</div>
			</div>
		</div>
	</div>
</template>

<script>
	export default {
		data() {
			return {
				email: '',
				password: '',
			};
		},
		methods: {
			register() {
				this.$store.dispatch('register', {
					email: this.email,
					password: this.password,
				});
				this.email = '';
				this.password = '';
			},
		},
	};
</script>

<style scoped>
	.register {
		margin-top: 20px;
		display: flex;
		flex-flow: column nowrap;
		justify-content: center;
		align-items: center;
	}
	.container {
		width: 500px;
		border:3px solid #ccc;
	}
	input {
		margin: 10px 0;
		padding: 10px;
	}
</style>
