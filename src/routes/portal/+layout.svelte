<script lang="ts">
	import { goto } from '$app/navigation';
	import { getZodErrorMessage, } from '$lib';
	import NavHr from '$lib/components/nav/navHr.svelte';
	import NavLinkCustom from '$lib/components/nav/navLinkCustom.svelte';
	import { Nav, NavItem, Button, Icon, Card, Dropdown, DropdownToggle, DropdownMenu, DropdownItem, Modal, ModalBody } from '@sveltestrap/sveltestrap';
	import { onMount, tick } from 'svelte';
	import { expoOut } from 'svelte/easing';
	import { fade, fly, slide } from 'svelte/transition';
	import { PrivilegeLevel } from '@prisma/client';
	import GeneralHr from '$lib/components/generalHr.svelte';
	import { ZodError } from 'zod';

	let { children } = $props();


	// let user: UserGet | null = $state(null);

	let completedLoginCheck: boolean = $state(true);


	let showUserEditModal: boolean = $state(false);
    // let userEditForm: UserForm;

	function openEditUser() {
		// const userDerived = user;
		// if (userDerived === null) throw new Error("user is null");

        showUserEditModal = true;

        // tick().then(() => {
        //     userEditForm.setResult({
        //         username: userDerived.username,
        //         email: userDerived.email,
        //         privilege: userDerived.privilege
        //     })
        // })
    }
    function closeEditUser() {
        showUserEditModal = false;
    }
    // async function submitEdits() {
    //     let formResult: DeepPartial<UserUpsert>;
    //     try {
    //         formResult = userEditForm.getResult();
    //     }
    //     catch (e) {
    //         if (e instanceof ZodError) {
    //             alert(getZodErrorMessage(e));
    //             return;
    //         }

    //         throw e;
    //     }

    //     if (user === null) throw new Error("No ID.");


    //     const result = await UserAPIRoute.instance.patch(user.id, formResult);
    //     if (!(await result.isOK())) {
    //         alert("An error occurred while processing your request.");
    //         return;
    //     }

    //     closeEditUser();
    //     alert("User edited. Please login again.")
	// 	goto("/");
    // }



	function onLogout() {
		goto("/");
		// if (confirm('Are you sure you want to logout?')) {
		// 	const result = makeLogoutRequest();
		// 	if (!result) {
		// 		alert('Failed to logout.');
		// 		return;
		// 	}

		// 	goto('/');
		// }
	}

	// onMount(() => {
	// 	user = getCurrentUser();
	// 	if (user === null) {
	// 		alert('You need to login first.');
	// 		goto('/?openLogin=true');
	// 		return;
	// 	}

	// 	completedLoginCheck = true;
	// });
</script>


<style>
	.z-100 {
		z-index: 100;
	}

	.sidebar {
		width: 350px;
	}
</style>


{#if completedLoginCheck}
	<div class="d-flex flex-row vw-100 vh-100">
		<div class="position-absolute z-100 h-100 d-flex flex-row">
			<div class="z-3 overflow-hidden bg-light">
				<div class="h-100 sidebar overflow-hidden">
					<Nav vertical class="d-flex flex-column w-100 h-100 p-3">
						<div class="d-flex flex-column w-100 h-100">
							<div class="d-flex flex-column">
								<div class="d-flex w-100 justify-content-center">
									<h1>BDDS</h1>
								</div>
	
								<NavHr />
							</div>
	
							<div class="d-flex flex-column h-100 overflow-auto">
								<h6>Basic Actions</h6>
								<NavItem>
									<NavLinkCustom route="/portal/dashboard" iconName="grid-1x2-fill" label="Dashboard" />
								</NavItem>
								<NavHr />

								<h6>Manage Documents</h6>
								<NavItem>
									<NavLinkCustom route="/portal/doc/upload" iconName="pencil" label="Upload Document" />
								</NavItem>
								<NavItem>
									<NavLinkCustom route="/portal/doc/edit" iconName="search" label="Edit Documents" />
								</NavItem>
								<NavItem>
									<NavLinkCustom route="/portal/doc/search" iconName="search" label="Search Documents" />
								</NavItem>

								{#if true } <!-- user?.privilege === PrivilegeLevel.ADMIN} -->
									<NavHr />

									<h6>Admin Tools</h6>
									<NavItem>
										<NavLinkCustom route="/portal/user" iconName="person-bounding-box" label="Users" />
									</NavItem>
								{/if}
							</div>

							<div class="d-flex flex-column">
								<Card class="d-flex flex-row justify-content-between align-content-center p-3">
									<h5 class="align-content-center m-0">
										<Icon name="person" class="m-0" />
										{#if true } <!-- user !== null} -->
											<span class="ms-2 m-0">user.username</span>
										{/if}
									</h5>

									<Dropdown dropup direction="up">
										<DropdownToggle color="primary" caret class="m-0">
											<Icon name="gear-fill" /> Settings
										</DropdownToggle>
										<DropdownMenu end>
											<DropdownItem header>user?.privilege ACCOUNT</DropdownItem>
											<DropdownItem header>User Actions</DropdownItem>
											<DropdownItem on:click={openEditUser}>Edit Account</DropdownItem>
											<DropdownItem on:click={onLogout}>Logout</DropdownItem>
										</DropdownMenu>
									</Dropdown>
								</Card>
							</div>
						</div>
					</Nav>
				</div>
			</div>
		</div>


		<div class="d-flex w-100 h-100">
			<div class="d-flex flex-column w-100 h-100 overflow-auto">
				<div class="d-none d-lg-flex flex-column w-100 overflow-auto" style="height: 100vh">
					{@render children()}
				</div>

				<div class="d-flex d-lg-none flex-column w-100 overflow-auto" style="height: 92vh">
					{@render children()}
				</div>
			</div>
		</div>
	</div>
{/if}



<Modal bind:isOpen={showUserEditModal}>
    <div class="d-flex w-100 p-2 justify-content-between align-items-center">
        <div class="d-flex w-100 justify-content-center">
            <h2 class="m-0 text-primary">EDITING USER</h2>
        </div>
        <Button
            color="white"
            class="p-0 m-0"
            on:click={closeEditUser}
        >
            <Icon name="x" class="h1" />
        </Button>
    </div>

	<ModalBody class="text-center position-relative">
		<!-- <UserForm disablePrivilege={true} bind:this={userEditForm}/> -->

        <GeneralHr />

        <!-- <Button color="primary" class="w-100" on:click={submitEdits}>Submit Edits</Button> -->
	</ModalBody>
</Modal>