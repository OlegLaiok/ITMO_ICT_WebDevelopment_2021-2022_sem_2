<template>
	<div class="container">
		<div class="mb-3">
			<button @click="tab=1" class="btn btn-primary mr-2">Administrators</button>
			<button @click="tab=2" class="btn btn-primary">Cleaners</button>
		</div>
		<div v-if="tab===1" class="">
			<b-form class="w-50 mx-auto" method='post'>
				<h2>Add new admin</h2>
				<b-form-group
					label="Login"
					label-for="username"
				>
					<b-form-input
						name="username"
						type="text"
						v-model="username"
						required
					/>
				</b-form-group>

				<b-form-group
					label="Full name"
					label-for="full_name"
				>
					<b-form-input
						name="full_name"
						type="text"
						v-model="full_name"
						required
					/>
				</b-form-group>

				<b-form-group
					label="password"
					label-for="password"
				>
					<b-form-input
						name="password"
						type="password"
						v-model="password"
						required
					/>
				</b-form-group>

				<b-form-group
					label="phone"
					label-for="phone"
				>
					<b-form-input
						name="phone"
						type="text"
						v-model="phone"
						required
					/>
				</b-form-group>

				<button @click="create_admin()" type="button" class="btn btn-success">Add</button>
			</b-form>
			<hr>
			<h2>Existing admins:</h2>
			<hr>
			<div v-for="admin in admins" :key="admin.id">
				<h3>{{admin.full_name}}</h3>
				<p>Phone: {{admin.phone}}</p>
				<button @click="delete_admin(admin.id)" type="button" class="btn btn-danger">Delete</button>
				<hr>
			</div>
		</div>

		<div v-if="tab===2" class="">
			<b-form class="w-50 mx-auto" method='post'>
				<h2>Add new cleaner</h2>

				<b-form-group
					label="ФИО"
					label-for="name"
				>
					<b-form-input
						name="name"
						type="text"
						v-model="name"
						required
					/>
				</b-form-group>

				<b-form-group
					label="phone"
					label-for="phone"
				>
					<b-form-input
						name="phone"
						type="text"
						v-model="phone"
						required
					/>
				</b-form-group>

				<button @click="create_cleaner()" type="button" class="btn btn-success">Add</button>
			</b-form>
			<hr>
			<h2>Existing cleaners:</h2>
			<hr>
			<div v-for="cleaner in cleaners" :key="cleaner.id">
				<h3>{{cleaner.name}}</h3>
				<p>Phone: {{cleaner.phone}}</p>
				<button @click="delete_cleaner(cleaner.id)" type="button" class="btn btn-danger">Delete</button>
				<hr>
			</div>
		</div>
	</div>
</template>

<script>
import axios from 'axios'
export default {
	name: 'Superuser',
	data () {
		return {
			admins: [],
			cleaners: [],
			username: '',
			full_name: '',
			name: '',
			password: '',
			phone: '',
			tab: 1
		}
	},
	created () {
		axios({
			method: 'get',
			url: 'http://127.0.0.1:8000/admins/',
			responseType: 'json',
			headers: { Authorization: 'Token ' + sessionStorage.getItem('auth_token') }
		}).then(response => {
			this.admins = response.data.results
		})

		axios({
			method: 'get',
			url: 'http://127.0.0.1:8000/staff/',
			responseType: 'json',
			headers: { Authorization: 'Token ' + sessionStorage.getItem('auth_token') }
		}).then(response => {
			this.cleaners = response.data.results
		})
	},
	methods: {
		delete_admin (id) {
			axios({
				method: 'delete',
				url: 'http://127.0.0.1:8000/admins/' + id + '/',
				responseType: 'json',
				headers: { Authorization: 'Token ' + sessionStorage.getItem('auth_token') }
			}).then(response => {
				document.location.reload()
			})
		},
		delete_cleaner (id) {
			axios({
				method: 'delete',
				url: 'http://127.0.0.1:8000/staff/' + id + '/',
				responseType: 'json',
				headers: { Authorization: 'Token ' + sessionStorage.getItem('auth_token') }
			}).then(response => {
				document.location.reload()
			})
		},
		create_admin () {
			axios({
				method: 'post',
				url: 'http://127.0.0.1:8000/admins/',
				responseType: 'json',
				data: {
					username: this.username,
					password: this.password,
					full_name: this.full_name,
					phone: this.phone
				},
				headers: { Authorization: 'Token ' + sessionStorage.getItem('auth_token') }
			}).then(response => {
				document.location.reload()
			}).catch(err => {
				console.log(err.response)
			})
		},
		create_cleaner () {
			axios({
				method: 'post',
				url: 'http://127.0.0.1:8000/staff/',
				responseType: 'json',
				data: {
					phone: this.phone,
					name: this.name
				},
				headers: { Authorization: 'Token ' + sessionStorage.getItem('auth_token') }
			}).then(response => {
				document.location.reload()
			}).catch(err => {
				console.log(err.response)
			})
		}
	}
}
</script>

<style>

</style>
