<script lang="ts">
	import { useToast } from '$lib/use-toast';
	import LoginWithGoogle from '@components/login-with-google.svelte';
	import { updateProfileEmail, useRelogin, useUser } from '$lib/user-user';

	const user = useUser();
	const toast = useToast();
	const relogin = useRelogin();

	let email: HTMLInputElement;

	const updateProfile = async () => {
		const { error } = await updateProfileEmail(email.value);
		if (error) {
			if (error === 'Firebase: Error (auth/requires-recent-login).') {
				relogin.set(true);
				return;
			}
			toast.error(error);
			return;
		}

		toast.open('Email Updated!');
	};
</script>

{#if $user}
	<main class="mt-5 flex flex-col items-center justify-center gap-5">
		<form class="flex flex-col items-center gap-5" on:submit|preventDefault={updateProfile}>
			{#if $relogin}
				<LoginWithGoogle />
			{:else}
				<div>
					<label for="email" class="mb-2 block text-sm font-medium text-gray-900 dark:text-white">
						Email
					</label>
					<input
						bind:this={email}
						type="text"
						id="email"
						name="email"
						value={$user.email}
						class="block w-full rounded-lg border border-gray-300 bg-gray-50 p-2.5 text-sm text-gray-900 focus:border-blue-500 focus:ring-blue-500"
						required
					/>
				</div>
				<button
					type="submit"
					class="w-fit rounded-lg border bg-blue-600 p-3 font-semibold text-white"
				>
					Update
				</button>
			{/if}
		</form>
	</main>
{:else}
	<LoginWithGoogle />
{/if}
