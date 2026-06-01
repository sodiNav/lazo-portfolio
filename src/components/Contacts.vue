<script setup>

    import { Notyf } from 'notyf';
    import { ref, onMounted, onBeforeUnmount } from 'vue';

    const notyf = new Notyf();

    const name = ref('');
    const email = ref('');
    const contactNo = ref('');
    const message = ref('');
    const isLoading = ref(false);

	const WEB3FORMS_ACCESS_KEY = "73264586-b11b-4fb7-8ceb-b7db13b6b115";

	const subject = "New message from Portfolio Contact Form";

	const submitForm = async () => {

		if(!recaptchaToken.value) {
			notyf.error('Please verify that you are not a robot');
			return;
		}

		isLoading.value = true;

		try {

			const response = await fetch("https://api.web3forms.com/submit", {
				method: "POST",
				headers: {
					"Content-Type": "application/json",
					Accept: "application/json"
				},
				body: JSON.stringify({
					access_key: WEB3FORMS_ACCESS_KEY,
					subject: subject,
					name: name.value,
					email: email.value,
                    contactNo: contactNo.value,
					message: message.value
				})
			})

			const result = await response.json();

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
			resetRecaptcha();
            name.value = '';
            email.value = '';
            contactNo.value = '';
            message.value = '';
		}

	}

    /*reCAPTCHA Integration*/

    const SITE_KEY = '6LdJZActAAAAAAgD9YJfVBvLP0XKlwweke1JQJ7l';  
    const recaptchaContainer = ref(null);
    const recaptchaWidgetId = ref(null);
    const recaptchaToken = ref('');

    function onRecaptchaSuccess(token) {
        recaptchaToken.value = token;
    }

    function onRecaptchaExpired() {
        recaptchaToken.value = '';
    }

    function renderRecaptcha() {
        if (!window.grecaptcha) {
            console.error('reCAPTCHA not loaded');
            return;
        }

        recaptchaWidgetId.value = window.grecaptcha.render(recaptchaContainer.value, {
            sitekey: SITE_KEY,
            size: 'normal',
            callback: onRecaptchaSuccess,
            'expired-callback': onRecaptchaExpired,
        });
    }

    function resetRecaptcha() {
        if (recaptchaWidgetId.value !== null) {
        	window.grecaptcha.reset(recaptchaWidgetId.value);
        	recaptchaToken.value = '';
        }
    }

    onMounted(() => {
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
                <form @submit.prevent="submitForm">
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
                        <input type="text" class="form-control" v-model="contactNo" id="contact_num">
                    </div>
                    <div class="mb-3">
                        <label for="message" class="form-label">Message:</label>
                        <textarea class="form-control" v-model="message" id="message" rows="3"></textarea>
                    </div>
                    <div class="d-flex justify-content-end">
                        <button type="submit" class="btn bg-secondary-custom text-light align-end" :disabled="isLoading">
                            <i class="bi bi-send-fill" :class="{ 'd-none': isLoading }"></i>
                            {{ isLoading ? 'Sending...' : `Send` }}
                        </button>
                    </div>

                    <div class="d-flex justify-content-end mt-2">
	                	<div ref="recaptchaContainer"></div>
	                </div>
                </form>
            </div>
        </div>
    </section>
    
    <!-- End of Contact Section -->

</template>

<style scoped></style>
