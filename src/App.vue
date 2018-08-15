<template>
	<v-app>
		<v-container>
			<v-layout column align-center>
				<v-card>
					<v-toolbar color="primary" dark>
						<v-toolbar-title>Password Generator</v-toolbar-title>
					</v-toolbar>
					<v-container>
						<v-slider :min="1" :max="255" label="Size" thumb-label="always" v-model="size" always-dirty></v-slider>
						<v-checkbox label="Capital letters" v-model="capitalLetters"></v-checkbox>
						<v-checkbox label="Small letters" v-model="smallLetters"></v-checkbox>
						<v-checkbox label="Numbers" v-model="numbers"></v-checkbox>
						<v-checkbox label="Special characters (!@#$%&*()-+.,;?{[}]^><:)" v-model="specialChars"></v-checkbox>
						<v-text-field ref="password" label="Password" append-icon="mdi-content-copy" @click:append="copy" v-model="password"></v-text-field>
					</v-container>
					<v-card-actions>
						<v-btn color="primary" @click="generatePassword">Generate Password</v-btn>
					</v-card-actions>
					<v-snackbar v-model="snackbar">
						Copied to the clipboard
						<v-btn flat @click="snackbar = false" color="pink">
							<v-icon>mdi-close</v-icon>
						</v-btn>
					</v-snackbar>
				</v-card>
			</v-layout>
		</v-container>
	</v-app>
</template>

 <script>
export default {
	data() {
		return {
			size: 8,
			capitalLetters: true,
			smallLetters: true,
			numbers: true,
			specialChars: false,
			password: '',
			snackbar: false
		}
	},
	methods: {
		generatePassword() {
			const range = (a, b) => Array.from({ length: b - a + 1 }, (n, i) => i + a)
			let charCodes = []
			let password = ''

			if (this.capitalLetters)
				charCodes = [...charCodes, ...range(65, 90)]

			if (this.smallLetters)
				charCodes = [...charCodes, ...range(97, 122)]

			if (this.numbers)
				charCodes = [...charCodes, ...range(48, 57)]

			if (this.specialChars)
				charCodes = [...charCodes, 33, 64, 35, 36, 37, 38, 42, 40, 41, 45, 43, 46, 44, 59, 63, 123, 91, 125, 93, 94, 62, 60, 58]

			for (let i = 0; i < this.size; i++)
				password += String.fromCharCode(charCodes[Math.round(Math.random() * (charCodes.length - 1))])

			this.password = password
		},
		copy() {
			this.$refs.password.$refs.input.select()
			document.execCommand('copy')
			this.snackbar = true
		}
	}
}
</script>
 