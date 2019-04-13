<template>
    <div class="ui centered grid" style="margin-top: 30px;">
        <div class="ui card">
            <div class="content">
                <div class="note-message">
                    <div>
                        Join <span style="font-size: 18px; font-weight: 900"> Japan Amazon Web Service Student Group</span>
                    </div>
                    <div>
                        <span style="color: #4d78ed;">{{ info.active }}</span> users online of <span style="color: #4d78ed;">{{ info.total }}</span> registered.
                    </div>
                </div>
                <div class="ui form">
                    <div class="ui fluid left icon input">
                        <input type="email" placeholder="12345678@university.ac.jp" class="your-email" @keypress.enter="sendEmail" v-model="email">
                        <i class="envelope icon"></i>
                    </div>
                    <div class="welcom-message" v-show="err==='start'">
                        <p>Join us with your University email addres 🚀🚀🚀🚀🚀</p>
                    </div>
                    <div class="ui negative message" v-show="err==='not-university'">
                        <p>Seems like you are not a student in Japan</p>
                    </div>
                    <div class="ui negative message" v-show="err==='error'">
                        <p>Please enter a valid email address</p>
                    </div>
                    <div class="ui positive message" v-show="err==='no-error'">
                        <p>Now you are ready to sign in, please click the below button and join us!</p>
                    </div>
                    <div class="ui teal message" v-show="err==='done'">
                        <p>Thank You! Please check your email to activate your account </p><meta charset="U+1F680">
                    </div>
                    <div class="fluid ui submit-button button" @click="sendEmail" v-bind:class="{ disabled: isDisabled }">GET MY INVITE&nbsp;&nbsp;>></div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import axios from 'axios'
    import validator from 'email-validator'
    export default {
        data(){
            return{
                email: '',
                err: 'start',
                warn: '',
                isDisabled: true
            }
        },
        watch: {
            email: function(val, oldval){
                if(validator.validate(val) === true){
                    this.err = 'no-error'
                    this.isDisabled = false
                }
            }
        },
        props: ['info'],
        methods: {
            async sendEmail() {
                if (this.email.trim().length === 0) {
                    this.err = 'error'
                    return
                }
                if(validator.validate(this.email) === false){
                    this.err = 'error'
                    return
                }
                try{
                    let params = new URLSearchParams();
                    params.append('token', process.env.SLACK_TOKEN);
                    params.append('email', this.email);
                    let {data} = await axios.post('https://slack.com/api/users.admin.invite', params);
                    this.err = 'done'
                }
                catch(err){
                    console.log(err)
                }
            }
        }
    }
</script>


<style scoped>
    .welcom-message{
        margin: 20px;
        color: white;
        font-weight: 700;
    }
    .input-form {
        margin-top: 100px;
    }
    .ui.card{
        width:87%;
        height: 60%;
        background-color: #222F3D;
    }
    
    .note-message {
        margin-bottom: 10px;
        color: white;
        font-weight: 400;
    }

    .ui.submit-button.button{
        background-color: #e26309;
        color: white;
    }
    
    span {
        font-weight: bold;
        font-size: 105%;
    }
</style>
