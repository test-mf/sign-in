<template>
	<div class="container">
		<form v-on:submit="onSubmit" class="form">
			<input value="test@test.ru" type="text" name="username" />
			<input value="123" type="password" name="password" />
			<button>sign</button>
		</form>
	</div>
</template>

<script>
import { isUserLogin, userSignIn } from "@test-mf/api";
import { invokeEvent } from "@test-mf/eventbus";

export default {
	name: "SignIn",
	mounted() {
		// todo: create utility package for work with routing
		isUserLogin() && history.pushState(null, null, "#/dashboard");
	},
	methods: {
		onSubmit(event) {
			event.preventDefault();

			const { username, password } = Object.fromEntries(
				new FormData(event.target).entries()
			);

			userSignIn(username, password).then(() => {
				invokeEvent("userSigned");
				history.pushState(null, null, "#/dashboard");
			});
		},
	},
};
</script>

<style>
.container {
	display: flex;
	justify-content: center;
}

.form {
	margin-top: 50px;
	display: flex;
	flex-direction: column;
	width: 250px;
	gap: 10px;
}

input,
button {
	padding: 10px;
	border-radius: 5px;
}
</style>
