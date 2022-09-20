<template>
    <div>
        <vcl-facebook></vcl-facebook>
        <vcl-instagram></vcl-instagram>
    </div>
</template>

<script>
import { VclFacebook, VclInstagram } from 'vue-content-loading';

export default {
    name: 'Loading',
    components: {
        VclFacebook,
        VclInstagram,
    },
}
</script>