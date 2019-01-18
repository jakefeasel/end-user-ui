<template>
    <fr-center-card :showLogo="true">
        <b-card-body slot="center-card-body">
            <iframe id="loginIframe" style="border: 0;width:290px;height:340px"></iframe>
        </b-card-body>
    </fr-center-card>
</template>

<script>
    import CenterCard from '@/components/utils/CenterCard';
    import AppAuthHelper from 'appauthhelper';

    export default {
        name: 'Login',
        components: {
            'fr-center-card': CenterCard
        },
        mounted () {
            if (sessionStorage.getItem('accessToken')) {
                this.completeLogin();
            } else {
                AppAuthHelper.iframeRedirect(document.getElementById('loginIframe'));
            }
        }
    };
</script>
