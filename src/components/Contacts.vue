<script setup>

    import { Notyf } from 'notyf';
    import { ref, onMounted, onBeforeUnmount } from 'vue';

    const notyf = new Notyf();

    const name = ref('');
    const email = ref('');
    const contactNo = ref('');
    const message = ref('');
    const isLoading = ref(false);

    // Web3Forms Access Key used to authenticate form submissions.
	const WEB3FORMS_ACCESS_KEY = "73264586-b11b-4fb7-8ceb-b7db13b6b115";

    // Email subject that will appear when a form submission is received.
	const subject = "New message from Portfolio Contact Form";

    // The submitForm() function handles the contact form submission.
	const submitForm = async () => {

		// Ensure the user completes the reCAPTCHA challenge before submitting the form.
        // Check if a reCAPTCHA token exists
        // recaptchaToken.value - stores the verification token returned by Google reCAPTCHA
		// if(!recaptchaToken.value) {
		// 	notyf.error('Please verify that you are not a robot');
		// 	// Stop the form submission process
		// 	return;
		// }

		// While the email is being sent, disable the button and change it text to "Sending..."
		isLoading.value = true;

		try {

			// fetch() API is a built-in JavaScript function used to send HTTP requests to a server.
			const response = await fetch("https://api.web3forms.com/submit", {
				method: "POST",
				headers: {
					"Content-Type": "application/json",
					// Indicates that the request accepts a JSON response.
					Accept: "application/json"
				},
				// Convert the form data into a JSON string and include the access key.
				body: JSON.stringify({
					access_key: WEB3FORMS_ACCESS_KEY,
					subject: subject,
					name: name.value,
					email: email.value,
                    contactNo: contactNo.value,
					message: message.value
				})
			})

			// Convert the API response into a JS Object
			const result = await response.json();

				// Check if the form submission was successful.
				if (result.success) {
					console.log(result);
					isLoading.value = false;
					notyf.success("Message Sent!");
				}
		} catch (error) {
			console.log(error);
			isLoading.value = false;
			notyf.error("Failed to send message.");
		} finally {
			// Reset the reCAPTCHA widget after the submission process completes, whether the request succeeds or fails.
			// resetRecaptcha();
		}

	}

    /*reCAPTCHA Integration*/

    const SITE_KEY = '6LdJZActAAAAAAgD9YJfVBvLP0XKlwweke1JQJ7l';  // Replace with your site key
    // The location where the reCAPTCHA checkbox will appear
    const recaptchaContainer = ref(null);
    // The ID of the reCAPTCHA widget after it is created
    const recaptchaWidgetId = ref(null);
    // Stores the token generated when the user completes reCAPTCHA
    const recaptchaToken = ref('');

    // Runs when the user successfully completes the reCAPTCHA challenge
    function onRecaptchaSuccess(token) {
        recaptchaToken.value = token;
    }

    // Runs when the reCAPTCHA verification expires
    // Token typically expires after about 2 minutes (120 seconds) if the form has not been submitted
    function onRecaptchaExpired() {
        recaptchaToken.value = '';
    }

    // Creates and displays the reCAPTCHA widget
    function renderRecaptcha() {
    	// Check if the Google reCAPTCH script is available
    	// window - refers to the browser window object
        if (!window.grecaptcha) {
            console.error('reCAPTCHA not loaded');
            return;
        }

        // Creates the reCAPTCHA widget and save its ID
        // Google returns a unique widget ID after creating the reCAPTCHA widget
            // render() generates the reCAPTCHA checkbox inside recaptchaContainer.value
            // Call window.grecaptcha.render() to create the widget, then store the widget ID returned by Google in recaptchaWidgetId.value.
        recaptchaWidgetId.value = window.grecaptcha.render(recaptchaContainer.value, {
            sitekey: SITE_KEY,
            size: 'normal',
            callback: onRecaptchaSuccess,
            'expired-callback': onRecaptchaExpired,
        });
    }

    // Function the resets the reCAPTCHA widget
    function resetRecaptcha() {
        if (recaptchaWidgetId.value !== null) {
        	window.grecaptcha.reset(recaptchaWidgetId.value);
        	recaptchaToken.value = '';
        }
    }

    onMounted(() => {
    // Check if the Google reCAPTCHA library has finished loading, since it is loaded asynchronously from index.html.
    // This makes sure that the reCAPTCHA widget is rendered only after the library is available.
    // setInterval() is a JavaScript function that repeatedly executes a block of code at a specified time interval.
    const interval = setInterval(() => {
        if (window.grecaptcha && window.grecaptcha.render) {
            renderRecaptcha();
            // Stop checking once the widget has been rendered
            clearInterval(interval);
        }
        // Check every 100 milliseconds if the Google reCAPTCHA library has loaded
    }, 100);

    // Run cleanup code before the component is removed
	onBeforeUnmount(() => {
            // Stop checking once the widget has been rendered
        clearInterval(interval);
        });
    });  

</script>

<template>
    <!-- Contact Section -->
    
    <section>
        <div id="contact" class="row bg-lime-custom p-3 p-md-5">
            <h3 class="text-center header-font mb-4 mb-lg-5">Contact</h3>
            <h4>Get in Touch</h4>
            <div id="contact-links" class="col-12 col-lg-6 d-flex flex-column">
                <span class="fs-5 py-2 text-center text-md-start align-items-center">
                    <i class="bi bi-envelope-fill"></i>
                    &#8195;jamesivnlazo@gmail.com
                </span>
                <div class="d-flex flex-row flex-md-column justify-content-center">
                    <a href="https://github.com/" class="text-decoration-none text-dark d-inline-block fs-5 py-2 d-flex align-items-center px-2 px-md-0">
                        <i class="bi bi-github"></i>
                        <span class="d-none d-md-inline-block">&#8195;GitHub</span>
                    </a>
                    <a href="https://about.gitlab.com/" class="text-decoration-none text-dark d-inline-block fs-5 py-2 d-flex align-items-center px-2 px-md-0">
                        <i class="bi bi-gitlab"></i>
                        <span class="d-none d-md-inline-block">&#8195;GitLab</span>
                    </a>
                    <a href="https://www.linkedin.com/" class="text-decoration-none text-dark d-inline-block fs-5 py-2 d-flex align-items-center px-2 px-md-0">
                        <i class="bi bi-linkedin"></i>
                        <span class="d-none d-md-inline-block">&#8195;LinkedIn</span>
                    </a>
                    <a href="https://www.facebook.com/" class="text-decoration-none text-dark d-inline-block fs-5 py-2 d-flex align-items-center px-2 px-md-0">
                        <i class="bi bi-facebook"></i>
                        <span class="d-none d-md-inline-block">&#8195;Facebook</span>
                    </a>
                    <a href="https://www.instagram.com/" class="text-decoration-none text-dark d-inline-block fs-5 py-2 d-flex align-items-center px-2 px-md-0">
                        <i class="bi bi-instagram"></i>
                        <span class="d-none d-md-inline-block">&#8195;Instagram</span>
                    </a>
                </div>
            </div>
            <div class="col-12 col-lg-6">
                <form action="">
                    <div class="mb-3">
                        <label for="fullName" class="form-label">Full Name:</label>
                        <input type="text" class="form-control" v-model="name" id="fullName">
                    </div>
                    <div class="mb-3">
                        <label for="Email" class="form-label">Email:</label>
                        <input type="email" class="form-control" v-model="email" id="Email">
                    </div>
                    <div class="mb-3">
                        <label for="contact_num" class="form-label">Contact Number:</label>
                        <input type="number" class="form-control" v-model="contactNo" id="contact_num">
                    </div>
                    <div class="mb-3">
                        <label for="message" class="form-label">Message:</label>
                        <textarea class="form-control" v-model="message" id="message" rows="3"></textarea>
                    </div>
                    <div class="d-flex justify-content-end">
                        <button type="submit" class="btn bg-primary-custom text-light align-end">
                            {{ isLoading ? 'Sending...' : `<i class="bi bi-send-fill"></i>  
                            Send` }}
                        </button>
                    </div>
                </form>
            </div>
        </div>
    </section>
    
    <!-- End of Contact Section -->

</template>

<style scoped></style>
