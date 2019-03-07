<template>
 <v-container>
   <v-layout row wrap>
     <v-flex xs12 sm12 md6>
       <div><h1>Connect with us</h1>
         Address:
         <p>3rd Floor, Savita complex, Club road, Muzaffarpur, 842001</p>
         <p>Phone # +91 8969768695<br />
           Email: drupalchamp@gmail.com<br />
           Skype: drupalchamp<br />
           Gtalk: drupalchamp<br />
         </p>
       </div>
     </v-flex>

     <v-flex xs12 sm12 md6>
	  <flash-message class="myCustomClass"></flash-message>
	  <v-form
	    ref="form"
	    v-model="valid"
	    lazy-validation
	  >
	    <v-text-field
	      v-model="name"
	      :counter="10"
	      :rules="nameRules"
	      label="Name"
	      required
	    ></v-text-field>

	    <v-text-field
	      v-model="email"
	      :rules="emailRules"
	      label="E-mail"
	      required
	    ></v-text-field>

	    <v-text-field
	      v-model="subject"
	      label="Subject"
	      required
	    ></v-text-field>

	    <v-text-field
	      label="Message"
	      v-model="message"
	      multi-line
	      :rows="5"
	      required
	    ></v-text-field>

	    <v-btn @click="submit">submit</v-btn>

	  </v-form>
	  </v-flex>
   </v-layout>
  </v-container>
</template>

<script>
  import axios from 'axios';

  import Vue from 'vue';
  import VueFlashMessage from 'vue-flash-message';
  require('vue-flash-message/dist/vue-flash-message.min.css');
  Vue.use(VueFlashMessage);

  export default {
    data () {
      return { 
        valid: false,
        name: '',
        nameRules: [
          (v) => !!v || 'Name is required',
          (v) => v.length <= 10 || 'Name must be less than 10 characters'
        ],
        email: '',
        emailRules: [
          (v) => !!v || 'E-mail is required',
          (v) => /^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/.test(v) || 'E-mail must be valid'
        ],
	subject: '',
	message: ''
      }
    },
    methods: {
      submit () {
	let self = this;

	axios.get('http://localhost/drupal8/rest/session/token')
	.then(result => {
          if (200 === result.status) {
                const csrfToken = result.data;
	        fetch('http://localhost/drupal8/contact_message?_format=json', {
		  method: 'POST',
		  headers: {
		    'Content-Type': 'application/json',
		    'X-CSRF-Token': csrfToken
		  },
		  body: JSON.stringify({
		    contact_form:[{"target_id": "feedback"}],
		    name:[{"value": this.name}],
		    mail:[{"value": this.email}],
		    subject:[{"value": this.subject}],
		    message:[{"value": this.message}]
		  }),
		})
		.then(response => {
		    self.flash('Your data is submitted successfully', 'success', {
		      timeout: 3000,
		    });
		})
		.catch(error => {
		    self.flash('Something Worng!, Please submit again.', 'error', {
		      timeout: 3000,
		    });
		});
          }

	})
	.catch(function (response) {
	    console.log(response);	
	});

      }
    }
  }
</script>