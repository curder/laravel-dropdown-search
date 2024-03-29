<script setup>
import {autocomplete} from "@algolia/autocomplete-js";
import {onMounted, ref} from "vue";
import {getMeilisearchResults, meilisearchAutocompleteClient} from "@meilisearch/autocomplete-client";
import {createLocalStorageRecentSearchesPlugin} from "@algolia/autocomplete-plugin-recent-searches";

const searchContainer = ref()
const searchClient = new meilisearchAutocompleteClient({
    url: 'http://127.0.0.1:7700',
    apiKey: '',
})
const createSearchesPlugin = createLocalStorageRecentSearchesPlugin({
    key: 'RECENT_SEARCHES',
    limit: 5,
    transformSource({source}) {
        return {
            ...source,
            onSelect({setIsOpen}) {
                setIsOpen(true)
            }
        }
    },
});
onMounted(() => {
    autocomplete({
        container: searchContainer.value,
        placeholder: "您想要搜索什么?",
        initialState: {
            query: new URL(window.location).searchParams.get('q')
        },
        autoFocus: true,
        openOnFocus: true,
        autoComplete: true,
        onSubmit({navigator, state}) {
            navigator.navigate({itemUrl: `/search?q=${state.query}`})
        },
        plugins: [createSearchesPlugin,],
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
                    getItemUrl({item}) {
                        return item.url
                    },
                    templates: {
                        header({html}) {
                            return html`
                                <span class="aa-SourceHeaderTitle">用户</span>
                                <span class="aa-SourceHeaderLine"></span>
                            `;
                        },
                        item({item, html, components}) {
                            return html`
                                <a class="flex items-center space-x-2" href="${item.url}">
                                    <img src="${item.avatar_url}" class="w-8 h-8 rounded-full"/>
                                    <span>
                                        ${components.Highlight({
                                            hit: item,
                                            attribute: 'name',
                                            tagName: 'em',
                                        })}
                                    </span>
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
                    getItemUrl({item}) {
                        return item.url
                    },
                    templates: {
                        header({html}) {
                            return html`
                                <span class="aa-SourceHeaderTitle">课程</span>
                                <span class="aa-SourceHeaderLine"></span>
                            `;
                        },
                        item({item, html, components}) {
                            return html`
                                <a class="flex items-center space-x-2" href="${item.url}">
                                    <span>
                                       ${components.Highlight({
                                           hit: item,
                                           attribute: 'title',
                                           tagName: 'em',
                                       })}
                                    </span>
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
    <div ref="searchContainer"></div>
</template>
