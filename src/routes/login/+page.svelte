<script lang="ts">
    // Check if there is an existing user
    import { user } from '$lib/client/auth';
    import { goto } from '$app/navigation';
    import { onMount } from 'svelte';

    user.subscribe((userStore) => {
        console.log('User: ', userStore);
        if (userStore?.displayName) {
            const searchParams = new URLSearchParams(window.location.search);
            goto(searchParams.get('redirect') ?? '/');
        }
    });

    import { GoogleAuthProvider, signInWithPopup, getAuth } from 'firebase/auth';
    async function GoogleSignIn() {
        const provider = new GoogleAuthProvider();
        provider.setCustomParameters({ hd: 'dlsu.edu.ph' });
        await signInWithPopup(getAuth(), provider);
        goto('/dashboard');
    }
</script>

<p>Please use your @dlsu.edu.ph email account to sign in.</p>
<button class="btn btn-primary" on:click={GoogleSignIn}>Login with your DLSU Account</button>
