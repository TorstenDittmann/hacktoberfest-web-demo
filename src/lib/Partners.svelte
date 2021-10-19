<script lang="ts">
    import {
        LogoAppwrite,
        LogoDeepsource,
        LogoDigitalOcean,
        LogoIntel,
    } from "../assets/logos";
</script>

<div class="partners">
    <img src={LogoDigitalOcean} alt="DigitalOcean Logo" />
    <img src={LogoAppwrite} alt="Appwrite Logo" />
    <img src={LogoIntel} alt="Intel Logo" />
    <img src={LogoDeepsource} alt="deepsource Logo" />
</div>

<style>
    .partners {
        display: flex;
        justify-content: center;
        margin: 2rem 0;
    }

    .partners img {
        height: 4rem;
        margin: 0 1rem;
    }
</style>
