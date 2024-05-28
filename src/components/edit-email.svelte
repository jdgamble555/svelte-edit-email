<script lang="ts">
	import { useToast } from '$lib/use-toast';
	import LoginWithGoogle from '@components/login-with-google.svelte';
	import { updateProfileEmail, updateUser, useUser } from '$lib/user-user';
	const user = useUser();
	const toast = useToast();
	let email: HTMLInputElement;

	const updateProfile = async () => {
		const { error } = await updateProfileEmail(email.value);
		if (error) {
			toast.error(error);
			return;
		}
		toast.open('Email Updated!');
	};
</script>

{#if $user}
	<main class="flex flex-col items-center justify-center gap-5 mt-5">
		<form class="flex flex-col items-center gap-5" on:submit|preventDefault={updateProfile}>
			<div>
				<label
					for="email"
					class="block mb-2 text-sm font-medium text-gray-900 dark:text-white"
				>
					Email
				</label>
				<input
					bind:this={email}
					type="text"
					id="email"
					name="email"
					value={$user.email}
					class="bg-gray-50 border border-gray-300 text-gray-900 text-sm rounded-lg focus:ring-blue-500 focus:border-blue-500 block w-full p-2.5"
					required
				/>
			</div>
			<button
				type="submit"
				class="p-3 font-semibold text-white bg-blue-600 border rounded-lg w-fit"
			>
				Update
			</button>
		</form>
	</main>
{:else}
	<LoginWithGoogle />
{/if}
