<script>
	import { authHandlers } from '../store/store';
    import '../styles/authenticate.css';
</script>

<main class="flex flex-col items-center justify-center">
	<div class="mt-12">
		<h1 class = "top">Login</h1>
        <div class = "butt">
            <button on:click={authHandlers.loginWithGoogle}>
                Sign In with Google
            </button>
        </div>
	</div>
</main>