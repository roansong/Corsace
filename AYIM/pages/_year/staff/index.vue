<template>
    <div class="staff-page">
        <div class="staff-page__title">
            MCA - {{ mca.year }}
        </div>

        <nuxt-link
            :to="`/${mca.year}/staff/comments`"
            class="staff-page__link"
        >
            Comments pending >
        </nuxt-link>
    </div>
</template>

<script lang="ts">
import { Vue, Component } from "vue-property-decorator";
import { State } from "vuex-class";

import { MCA } from "../../../../Interfaces/mca";

@Component({
    head () {
        return {
            title: "Staff | AYIM",
        };
    },
})
export default class Staff extends Vue {

    @State mca!: MCA;

}
</script>
