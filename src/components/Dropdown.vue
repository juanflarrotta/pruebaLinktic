<template>
    <section class="dropdown">
        <span :class="`dropdown__arrow ${active ? 'dropdown__arrow--active' : ''}`">
            <svg width="12" height="7" viewBox="0 0 12 7" xmlns="http://www.w3.org/2000/svg">
                <path
                    d="M6.00001 6.50001C5.8684 6.50077 5.73794 6.47554 5.6161 6.42578C5.49427 6.37601 5.38345 6.30269 5.29001 6.21001L1.29 2.21001C1.19676 2.11677 1.1228 2.00608 1.07234 1.88426C1.02188 1.76243 0.995911 1.63187 0.995911 1.50001C0.995911 1.36815 1.02188 1.23758 1.07234 1.11576C1.1228 0.993934 1.19676 0.883244 1.29 0.790006C1.38324 0.696767 1.49393 0.622806 1.61575 0.572346C1.73758 0.521885 1.86814 0.495914 2 0.495914C2.13186 0.495914 2.26243 0.521885 2.38425 0.572346C2.50608 0.622806 2.61677 0.696767 2.71001 0.790006L6.00001 4.10001L9.30002 0.920006C9.39201 0.817716 9.50412 0.735507 9.62933 0.678521C9.75454 0.621535 9.89017 0.590998 10.0277 0.588819C10.1653 0.586639 10.3018 0.612864 10.4288 0.665855C10.5557 0.718846 10.6704 0.797463 10.7656 0.896788C10.8607 0.996112 10.9344 1.11401 10.9819 1.2431C11.0295 1.3722 11.0499 1.50971 11.0419 1.64705C11.0338 1.78438 10.9976 1.91859 10.9353 2.04126C10.873 2.16394 10.7861 2.27245 10.68 2.36001L6.68001 6.22001C6.49714 6.39632 6.25401 6.49643 6.00001 6.50001Z"
                    fill="#A0AEC0" />
            </svg>
        </span>
        <input type="text" v-model="search" @blur="desactivar" @input="filterFruit()" placeholder="Select an item"
            class="dropdown__input" @focus="activar">
        <ul v-show="active" id="listfruits" class="dropdown__list">
            <li v-for="fruit in newFruits" class="dropdown__item" v-on:click="change(fruit)">
                {{ fruit }}
            </li>
        </ul>
    </section>
</template>

<script lang="ts">
import { onMounted, reactive, ref } from 'vue';
import { CONSTANTS } from '../utils/constants';
import { fetchData } from '../utils/fetchData';

export default {
    name: "Dropdown",

    setup() {
        let active = ref(false);
        let newFruits = reactive([]);
        let search = ref('');
        let array: any[] = [];

        onMounted(async () => {
            const { data } = await fetchData(CONSTANTS.urlApiFruits);
            array = data.fruits;
            const fruits = data.fruits.map((element: string) => newFruits.push(element));
        });

        function filterFruit() {
            newFruits.splice(0);
            const Fruits = array.filter((fruit) => {
                if (fruit.includes(search.value)) {
                    newFruits.push(fruit);
                }
            });

            if (newFruits.length === 0) {
                newFruits.push('No items were found.');
            }

        };

        function change(val: string) {
            search.value = val;
            filterFruit();
            desactivar();
        };

        function activar() {
            active.value = true;
        };

        function desactivar() {
            setTimeout(() => {
                active.value = false;
            }, 150);
        }

        return { newFruits, search, filterFruit, change, active, activar, desactivar }
    },

}

</script>


<style scoped lang="scss">
@import '../utils/scss/variables.scss';

.dropdown {
    padding: 32px;
    display: flex;
    flex-direction: column;
    position: relative;

    &__arrow {
        position: absolute;
        top: 44px;
        right: 48px;

        &--active {
            transform: rotate(180deg);

            svg {
                path {
                    fill: $black;
                }
            }
        }
    }

    &__list {
        color: $black;
        box-shadow: 0px 10px 15px rgb(35 78 82 / 10%);
        background-color: $white;
        border-radius: 0 0 8px 8px;
        padding: 16px;
        margin-top: -5px;
        max-height: calc(100vh - 110px);
        overflow: auto;

        &::-webkit-scrollbar {
            display: none;
        }
    }

    &__item {
        font-size: 14px;
        cursor: pointer;
        text-transform: capitalize;

        &:hover {
            color: $blue;
        }
    }

    &__input {
        border: 0;
        padding: 16px;
        width: 100%;
        height: 100%;
        border-radius: 8px;
        border-color: $white;
        background-color: $white;
        color: $black;
        font-size: 16px;
        text-transform: capitalize;

        &::placeholder {
            color: $gray;
        }

        &:focus-visible {
            outline: none;
        }

        &::-webkit-scrollbar {
            display: none;
        }
    }
}
</style>
