<template>
  <form class="google-sheets-form" @submit.prevent="onSubmit">
    <textarea name="feedback" class="google-sheets-form__input google-sheets-form__input--textarea" placeholder="Enter here your feedback (optional)" data-gsp-name="Feedback (optional)" :data-gsp-data="data_feedback" v-model="data_feedback"/>
    <input name="email" class="google-sheets-form__input" type="email" placeholder="Your email (optional)" data-gsp-name="Email (optional)" 
    :data-gsp-data="data_email" v-model="data_email">

    <vue-recaptcha
      ref="recaptcha"
      size="invisible"
      @verify="onVerify"
      :sitekey="captcha">
    </vue-recaptcha>

    <div class="google-sheets-form__actions">
      <button v-if="!isSent" :class="classList" :disabled="error">
        <span v-if="!isLoading">Send Feedback</span>
        <span v-else>Sending Feedback...</span>
        <span class="spinner">
          <Spinner v-if="isLoading"/>
        </span>
      </button>
      <button disabled v-else class="button google-sheets-form__button">Feedback was sent, thanks</button>
      <div v-if="error" class="error">{{errorMessage}}</div>
    </div>
  </form>
</template>


<script>
export default {

  components: {
    Spinner: () => import("~/components/Spinner.vue"),
    VueRecaptcha: () => import("vue-recaptcha")
  },


  metaInfo: {
    script: [
      {
        src: 'https://www.google.com/recaptcha/api.js?onload=vueRecaptchaApiLoaded&render=explicit',
        body: true
      }
    ]
  },

  props: {
    response: {
      type: String,
      required: true,
      default: null,
      validator: function (value) {
        return ['smile', 'worry_grin', 'rolling_eyes', 'no_reaction'].indexOf(value) !== -1;
      }
    }
  },

  data() {
    return {
      gscript:  process.env.GRIDSOME_GSCRIPTID,
      captcha:  process.env.GRIDSOME_CAPTCHAID,
      // "6LeIxAcTAAAAAJcZVRqyHh71UMIEGNQ_MXjiZKhI" //test localhost
      data_feedback: '',
      data_email: '',
      isSent: false,
      isLoading: false,
      error: false,
      errorMessage: ''
    };
  },

  methods: {
    onSubmit: async function () {

      if (this.data_email === '' && this.data_feedback === '' && this.response === 'no_reaction') {
        this.error = true;
        this.errorMessage = 'Please, add some feedback or choose one of reaction emoji';

        return
      }

      this.$refs.recaptcha.execute();
    },

    onVerify: async function() {
      let response = '' ;
      const data = document.querySelectorAll('[data-gsp-data]');

      this.isSent = false;
      this.isLoading = true;
      this.error = false;

      data.forEach(function(item) {
        if (response != '') {
          response += '&'
        }
        response += encodeURIComponent(item.dataset.gspName) + '=' + encodeURIComponent(item.dataset.gspData)
      });

      const fullResponse = 'Reaction=' + encodeURIComponent(this.response) + '&Location=' + encodeURIComponent('https://wiki.robonomics.network' + this.$route.path) + '&' + response;

      try {
        await this.$gspPostForm(this.gscript, fullResponse)
        this.isLoading = false;
        this.isSent = true;
        this.error = false;
      } catch(e) {
        this.isSent = false;
        this.isLoading = false;
        this.error = true;
        this.errorMessage = e.message
        console.log(e.message);
      }
    }
  },

  computed: {
    classList() {
      return {
        [`button google-sheets-form__button`]: true,
        [`google-sheets-form__button--${this.response}`]: this.response,
      };
    },
  },

  watch: {
    'response': function(curr, old) {
      if (curr !== 'no_reaction') {
        this.error = false;
      }
    },

    'data_feedback': function(curr, old) {
      if (curr !== '') {
        this.error = false;
      }
    },

    'data_email': function(curr, old) {
      if (curr !== '') {
        this.error = false;
      }
    }
  }

}

</script>

<style scoped>
  .google-sheets-form {
    position: relative;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
    justify-content: center;
    transition: background-color 0.3s ease-in-out, color 0.3s ease-in-out;
  }

  .google-sheets-form__input {
    width: 100%;
    margin-bottom: 20px;
    padding: 10px 15px !important;
    font-size: 1rem;
    color: var(--text-color);
    text-transform: none;
    background-color: transparent;
    border: 1px solid var(--text-color);
    border-radius: 0%;
  }

  .google-sheets-form__input--textarea {
    min-height: 100px;
    resize: none;
  }

  .google-sheets-form__input::placeholder {
    font-size: 14px;
    text-transform: none;
    opacity: 0.7;
  }

  .google-sheets-form__actions {
    display: flex;
    align-items: center;
  }

  .google-sheets-form__button {
    padding: 10px 40px;
    display: flex;
    align-items: center;
    border: 1px solid transparent;
    background-color: var(--link-color);
    color: var(--text-color-invert);
    transition: background-color 0.2s ease-in-out;
  }

    .google-sheets-form__button:disabled {
      background-color: var(--color-note-accent);
    }

  .spinner svg{
    width: 40px;
    height: 30px;
  }

  .google-sheets-form__button--smile  {background-color: var(--color-emoji-green);}
  .google-sheets-form__button--rolling_eyes {background-color: var(--color-emoji-pink);}
  .google-sheets-form__button--worry_grin {background-color: var(--color-emoji-orange);}

  .error {
    margin-left: 25px;
    color: rgb(190, 50, 50);
    font-weight: 600;
    font-size: 1rem;
  }


</style>
