<template>
	<div class="container mt-5">
		<form class="row justify-content-center" @submit.prevent="login">
			<div class="col-md-6">
				<h1 class="h3 mb-3 font-weight-normal">請先登入</h1>
				<div class="mb-2">
					<label for="inputEmail" class="sr-only">Email address</label>
					<input
						type="email"
						id="inputEmail"
						class="form-control"
						placeholder="Email address"
						required
						autofocus
						v-model="user.username"
					/>
				</div>
				<div class="mb-2">
					<label for="inputPassword" class="sr-only">Password</label>
					<input
						type="password"
						id="inputPassword"
						class="form-control"
						placeholder="Password"
						required
						v-model="user.password"
					/>
				</div>

				<div class="text-end mt-4">
					<button class="btn btn-lg btn-primary btn-block" type="submit">登入</button>
				</div>
			</div>
		</form>
	</div>
</template>

<script>
export default {

	data() {
		return {
			user: {
				username: '',
				password: '',
			},
		}
	},
	methods: {
		login() {
			console.log(JSON.stringify(this.user))
			const api = `${process.env.VUE_APP_API}admin/signin`;
			fetch(api,
				{
					method: 'post',
					headers: {
						'Content-Type': 'application/json',
					},
					// 傳送內容須轉換為字串
					body: JSON.stringify(this.user),
				})
				.then((response) => {
					return response.json();
				})
				.then((data) => {
					if(data.success){
						console.log(data);
						document.cookie = `hexToken=${data.token}; expires=${new Date(data.expired)};`;
						this.$router.push('/dashboard');
					}
				})
				.catch((error) => {
					console.log(error);
				});
		},
	},
	mounted() {
		console.log(process.env)
	}
}
</script>
