<script setup>
	
	import { ref, onBeforeUnmount, onMounted } from 'vue';
	import { Notyf } from 'notyf';
	import axios from 'axios';

	const notyf = new Notyf({
		position: {
			x: 'center',
			y: 'bottom'
		}
	})

	const WEB3FORMS_ACCESS_KEY = "f93040ed-5e70-4132-9b77-fc7a1acefa7f";
	const SITE_KEY = "6LcBkBouAAAAALp4MMqtGzH-jVQYLnVggQSUC5kK";


	// Initial value for the email:
	const subject = "New message from Portfolio Contact Form";

	const name = ref('');
	const email = ref('');
	const message = ref('');

	const isLoading = ref(false);


	// Function for sending our email:
	const submitForm = async() => {

		if (!recaptchaToken.value) {
		        notyf.error('Please verify that you are not a robot.');
		        return;
		    }

		isLoading.value = true;

		try {

			const response = await axios.post('https://api.web3forms.com/submit', {
				access_key: WEB3FORMS_ACCESS_KEY,
				subject: subject,
				name: name.value,
				email: email.value,
				message: message.value
			},{
				headers: {
					'Content-Type' : "application/json",
					Accept: "application/json"
				}
			})

			const result = response.data;

			if (result.success){
				notyf.success('Message Sent!');

				name.value = "";
				email.value = "";
				message.value = "";
			}

		} catch (err) {

			notyf.error('Failed to send email');

		} finally {
			isLoading.value = false;
		}
	}





	const recaptchaContainer = ref(null);
	const recaptchaWidgetId = ref(null);
	const recaptchaToken = ref('');

	// Callback called by reCAPTCHA when successful
	function onRecaptchaSuccess(token) {
	  recaptchaToken.value = token;
	}

	// Callback when expired
	function onRecaptchaExpired() {
	  recaptchaToken.value = '';
	}

	// Function to render the reCAPTCHA widget
	function renderRecaptcha() {
	  if (!window.grecaptcha) {
	    console.error('reCAPTCHA not loaded');
	    return;
	  }

	  recaptchaWidgetId.value = window.grecaptcha.render(recaptchaContainer.value, {
	    sitekey: SITE_KEY,
	    size: 'normal', // or 'compact'
	    callback: onRecaptchaSuccess,
	    'expired-callback': onRecaptchaExpired,
	  });
	}

	// Function to reset reCAPTCHA 
	function resetRecaptcha() {
	  if (recaptchaWidgetId.value !== null) {
	    window.grecaptcha.reset(recaptchaWidgetId.value);
	    recaptchaToken.value = '';
	  }
	}



	onMounted(() => {
	  // This code waits for the Google reCAPTCHA library to load, then renders the reCAPTCHA widget using onMounted hook. 
	  // The widget is rendered with grecaptcha.render(), which requires a sitekey. 
	  // Callback functions handle success and expiration events. 
	  // reCAPTCHA is reset upon form submission to clear the token.
	  const interval = setInterval(() => {
	    if (window.grecaptcha && window.grecaptcha.render) {
	      renderRecaptcha();
	      clearInterval(interval);
	    }
	  }, 100);

	  onBeforeUnmount(() => {
	    clearInterval(interval);
	  });
	});

</script>

<template>
	
	<!-- contact start -->
	<div class="container-fluid" id="contact">
		<div class="row" id="h1-contact">
			<h1>Contact Us</h1>
		</div>
		<div class="row" id="row-contact">
			<div class="col-12 col-md-6" id="column-contact">
				<h2>MAP</h2>
				<div class="inner-contact">
					<iframe class="map-contact" src="https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d81106.54027733703!2d126.17007166360038!3d9.08509109239183!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x330237003488f0ff%3A0x988c2e7928928c9a!2sRIVER%20VIEW%20FOOD%20HOUSE!5e0!3m2!1sen!2sph!4v1790697454785!5m2!1sen!2sph" width="600" height="450" style="border:0;" allowfullscreen="" loading="lazy" referrerpolicy="strict-origin-when-cross-origin"></iframe>
				</div>
			</div>
			<form @submit.prevent="submitForm">
				<div class="col-12 col-md-6" id="column-contact">
					<h2>FORM</h2>
					<div class="inner-contact">
						<label>Name:</label>
						<input type="text" v-model="name" name="" placeholder="Name" required>
						<label>Email:</label>
						<input type="email" v-model="email" name="" placeholder="Email" required>
						<label>Message:</label>
						<textarea required v-model="message">
							
						</textarea>
						<button :disabled="isLoading">{{isLoading ? 'Sending...' : 'Send'}}</button>
					</div>
				</div>

				<!-- recaptcha checkbox -->
				<div class="d-flex justify-content-end mt-2">
				    <div ref="recaptchaContainer"></div>
				</div>

			</form>
		</div>
	</div>
	<!-- contact end -->
	
</template>