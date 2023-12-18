<script setup>
import {autocomplete} from "@algolia/autocomplete-js";
import {onMounted} from "vue";

onMounted(() => {

    autocomplete({
        container: "#search",
        placeholder: "您向要搜索什么?",
        autoFocus: true,
        getSources() {
            return [
                {
                    sourceId: 'users',
                    getItems({query}) {
                        let items = [
                            {id: 1, name: 'Curder'},
                            {id: 2, name: 'Li Lei'},
                        ];
                        return items.filter(({name}) => name.toUpperCase().includes(query.trim().toUpperCase()));
                    },
                    templates: {
                        item({item}) {
                            return item.name;
                        }
                    }
                }
            ];
        },
    })
})
</script>

<template>
    <div id="search"></div>
</template>
