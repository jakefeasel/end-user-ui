<template>
    <fr-center-card :showLogo="true">
        <b-card-body slot="center-card-body">
            <b-form class="form-signin mb-3" @submit.prevent="submit">
            </b-form>
        </b-card-body>
    </fr-center-card>
</template>

<script>
    import FloatingLabelInput from '@/components/utils/FloatingLabelInput';
    import CenterCard from '@/components/utils/CenterCard';
    import styles from '@/scss/main.scss';
    import ForgeRockEmbeddedLogin from 'forgerockembeddedlogin';

    var login = new ForgeRockEmbeddedLogin('https://login.sample.forgeops.com/json/realms/root/authenticate');

    export default {
        name: 'Login',
        components: {
            'fr-floating-label-input': FloatingLabelInput,
            'fr-center-card': CenterCard
        },
        data () {
            return {
                pageData: {},
                wrongPasswordSubmitted: false,
                loadingColor: styles.baseColor
            };
        },
        created () {
            login.startLogin()
            .then(this.handleLoginResponse);
        },
        methods: {
            handleLoginResponse () {
                if (login.success()) {
                    this.completeLogin();
                } else if (login.failure()) {
                    this.displayNotification('error', 'Login Failed! Try Again');
                    login.startLogin().then(this.render);
                } else {
                    this.render();
                }
            },
            render () {
                login.renderLogin().then((loginPrompt) => {
                    document.getElementsByClassName('form-signin mb-3')[0].innerHTML = loginPrompt;
                });
            },
            submit (e) {
                return login.handleLoginSubmit(e).then(this.handleLoginResponse);
            }
        }
    };
</script>
