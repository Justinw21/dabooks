<script>
	import { authHandlers } from '../store/store';
	import '../styles/authenticate.css';
</script>

<main class="flex flex-col items-center justify-center">
	<div class="container">
		<div class="mt-12">
			<h1 class="top">User Login</h1>
			<div class="butt">
				<button on:click={authHandlers.loginWithGoogle}>
					<i style="padding: 10px;" class="fa-brands fa-google" /> Sign In with Google
				</button>
			</div>
		</div>
	</div>
</main>
