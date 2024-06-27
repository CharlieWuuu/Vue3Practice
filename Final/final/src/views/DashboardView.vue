<template>
  Dashboard
</template>

<script>
export default {
  created() {
    console.log('dashboard 的元件生命週期');
    const token = document.cookie.replace(/(?:(?:^|.*;\s*)hexToken\s*=\s*([^;]*).*$)|^.*$/, "$1");

    const api = `${process.env.VUE_APP_API}api/user/check`;
    fetch(api,
				{
					method: 'post',
					headers: {
            'Authorization': token,
            'Content-Type': 'application/json' // 設置內容類型
          },
          body: JSON.stringify(this.user),
				})
				.then((response) => {
					return response.json();
				})
				.then((data) => {
          console.log(data);
          if(!data.success) {
            this.$router.push('/login');
          }
				})
				.catch((error) => {
          console.log(error);
          this.$router.push('/login');
				});
  }
}</script>
