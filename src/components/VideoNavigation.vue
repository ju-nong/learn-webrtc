<template>
    <div>
        <fontIcon :icon="iconConfig[type][power]" />
        <fontIcon :icon="faEllipsisVertical" />
    </div>
</template>

<script setup>
import { defineProps, computed, reactive } from "vue";
import {
    faVideo,
    faVideoSlash,
    faMicrophone,
    faMicrophoneSlash,
    faEllipsisVertical,
} from "@fortawesome/free-solid-svg-icons";

const props = defineProps({
    type: String,
    list: Array,
    power: Boolean,
});

const iconConfig = reactive({
    cam: {
        on: faVideo,
        off: faVideoSlash,
    },
    mic: {
        on: faMicrophone,
        off: faMicrophoneSlash,
    },
});
</script>

<style lang="scss" scoped>
div {
    display: flex;
    justify-content: space-evenly;
    width: 80px;
    height: 40px;

    border-radius: 2rem;
    border: 1px solid #000;

    button {
        all: unset;
        cursor: pointer;
        width: 50%;
        height: 100%;
        text-align: center;
    }
}
</style>
