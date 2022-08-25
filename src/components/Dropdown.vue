<template>
    <section class="dropdown">

        <Icono :nameClass="`dropdown__arrow ${active ? 'dropdown__arrow--active' : ''}`" :icono=iconoArrow />

        <input type="text" v-model="search" @blur="desactivar" @input="filterFruit()" placeholder="Select an item"
            class="dropdown__input" @focus="activar">

        <ul v-show="active" class="dropdown__list">
            <li v-for="fruit in newFruits" class="dropdown__item" v-on:click="change(fruit)">
                {{ fruit }}
            </li>
        </ul>

    </section>
</template>

<script lang="ts">
import { onMounted, reactive, ref } from 'vue';
import { CONSTANTS, ICONOS } from '../utils/constants';
import { fetchData } from '../utils/fetchData';
import Icono from './Icono.vue';

export default {
    name: "Dropdown",
    components: { Icono },

    data() {
        return {
            iconoArrow: ICONOS.arrow,
        }
    },

    setup() {
        let active = ref(false);
        let newFruits: any[] = reactive([]);
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

            ::v-deep {
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
