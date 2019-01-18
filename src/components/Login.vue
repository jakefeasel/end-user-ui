<template>
    <fr-center-card :showLogo="true">
        <div slot="center-card-header">
            <h2 class="h2">{{pageData.header}}</h2>
        </div>

        <b-card-body slot="center-card-body">
            <b-form  v-if="pageData.callbacks" class="form-signin mb-3" @submit.prevent="submit">
                <template  v-for="(callback) in pageData.callbacks">
                    <template  v-for="(input, index) in callback.input">
                        <fr-floating-label-input v-if="callback.type === 'PasswordCallback'" v-model="input.value" :fieldName="callback.output[index].value" :label="callback.output[index].value" type="password" autofocus="true"></fr-floating-label-input>

                        <fr-floating-label-input v-else v-model="input.value" :fieldName="callback.output[index].value" :label="callback.output[index].value" type="text" autofocus="true"></fr-floating-label-input>
                    </template>
                </template>
                <b-button type="submit" variant="primary" class="btn btn-block btn-lg">
                    {{$t('pages.login.signIn')}}
                </b-button>
            </b-form>

            <bounce-loader v-else :color="loadingColor"></bounce-loader>
        </b-card-body>
    </fr-center-card>
</template>

<script>
    import FloatingLabelInput from '@/components/utils/FloatingLabelInput';
    import CenterCard from '@/components/utils/CenterCard';
    import { BounceLoader } from 'vue-spinner/dist/vue-spinner.min.js';
    import styles from '@/scss/main.scss';
    import ForgeRockEmbeddedLogin from 'forgerockembeddedlogin';

    var login = new ForgeRockEmbeddedLogin('https://login.sample.forgeops.com/json/realms/root/authenticate');

    /**
     * @description Controlling component to allow users to manually login, socially login or start of a selfservice process (username, password or registration) if configured.
     *
     * @fires POST authentication?_action=logout - Ends current user session
     * @fires POST authentication?_action=login - Uses username and password to establish a new user session
     * @fires GET type/name/id (e.g. managed/user/_id) - Resource details, in this context it is for the successfully logged in user
     * @fires POST privilege?_action=listPrivileges - Check to see if a user has any privilege based access
     * @fires GET schema/type/name/ (e.g. schema/managed/user) - Schema for a resource
     *
     */
    export default {
        name: 'Login',
        components: {
            'fr-floating-label-input': FloatingLabelInput,
            'fr-center-card': CenterCard,
            BounceLoader
        },
        data () {
            return {
                pageData: {},
                wrongPasswordSubmitted: false,
                loadingColor: styles.baseColor
            };
        },
        created () {
            this.loadData();
        },
        methods: {
            handleCallbacks (callbacks) {
                if (login.success()) {
                    this.completeLogin();
                } else {
                    this.pageData = callbacks;
                }
            },
            loadData () {
                login.startLogin()
                .then(this.handleCallbacks);
            },
            submit () {
                return login.submitCredentials(this.pageData.callbacks.map((callback) => callback.input[0].value))
                .then(this.handleCallbacks);
            }
        }
    };
</script>
