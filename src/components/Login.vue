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
        },
        methods: {
            render () {
            }
        }
    };
</script>
