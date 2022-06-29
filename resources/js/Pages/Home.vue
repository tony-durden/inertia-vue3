<template>
    <div>
        <h1>Inertiajs is {{ test }}</h1>

        <Link href="/about-us">Go to about us</Link>
        <Tony />
    </div>
</template>

<script>
import { Link } from "@inertiajs/inertia-vue3";
import Tony from '../Components/Tony.vue';
export default {
    components: {
        Link,
        Tony
    },
    props: ["test"],
};
</script>