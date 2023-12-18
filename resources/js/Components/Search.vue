<script setup>
import {autocomplete} from "@algolia/autocomplete-js";
import {onMounted} from "vue";
import {getMeilisearchResults, meilisearchAutocompleteClient} from "@meilisearch/autocomplete-client";

const searchClient = new meilisearchAutocompleteClient({
    url: 'http://127.0.0.1:7700',
    apiKey: '',
})

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
                        return getMeilisearchResults({
                            searchClient,
                            queries: [
                                {
                                    indexName: 'users',
                                    query,
                                }
                            ]
                        })
                    },
                    templates: {
                        header({html}) {
                            return html`
                                <span class="aa-SourceHeaderTitle">用户</span>
                                <span class="aa-SourceHeaderLine"></span>
                            `;
                        },
                        item({item, html}) {
                            return html`
                                <a class="flex items-center space-x-2" href="">
                                    <img src="${item.avatar_url}" class="w-8 h-8 rounded-full"/>
                                    <span>${item.name}</span>
                                </a>
                            `;
                        }
                    }
                },
                {
                    sourceId: 'courses',
                    getItems({query}) {
                        return getMeilisearchResults({
                            searchClient,
                            queries: [
                                {
                                    indexName: 'courses',
                                    query,
                                }
                            ]
                        })
                    },
                    templates: {
                        header({html}) {
                            return html`
                                <span class="aa-SourceHeaderTitle">课程</span>
                                <span class="aa-SourceHeaderLine"></span>
                            `;
                        },
                        item({item, html}) {
                            return html`
                                <a class="flex items-center space-x-2" href="">
                                    <span>${item.title}</span>
                                </a>
                            `;
                        }
                    }
                },
            ];
        },
    })
})
</script>

<template>
    <div id="search"></div>
</template>
