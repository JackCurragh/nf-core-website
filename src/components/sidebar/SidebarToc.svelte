<script lang="ts">
    import { currentHeading, showHidden } from '@components/store';
    import { onMount } from 'svelte';

    export let headings: {
        text: string;
        slug: string;
        depth: number;
        fa_icon?: string;
        hidden?: boolean;
    }[];

    const min_heading_depth = Math.min(...headings.map((h) => h.depth));
    // make margin classes from min to max heading depth
    let headingMargin = {};
    for (let i = min_heading_depth; i <= 4; i++) {
        headingMargin[i] = 'ps-' + (i - min_heading_depth) * 2;
    }
    onMount(() => {
        // set the first heading as active on initial load
        if (!$currentHeading) {
            currentHeading.set(headings[0]?.slug);
        }
    });
</script>

{#if headings.length > 1}
    <div class="nav flex-column sticky-top-under align-items-end">
        <div class="d-none d-md-inline">
            <strong class="h6 my-2 text-body">On this page</strong>
            <!-- <hr class="my-1" /> -->
            <nav id="TableOfContents" class="d-none d-md-flex flex-column">
                <ul class="mb-0 mt-1">
                    {#each headings as heading (heading)}
                        <li
                            class={'nav-item ' + headingMargin[heading.depth]}
                            class:active={heading.slug === $currentHeading}
                            class:collapse={heading.hidden && !$showHidden}
                        >
                            <a class="nav-link py-1 ps-3 text-body-secondary" href={'#' + heading.slug}>
                                {#if heading.fa_icon}
                                    <i class={heading.fa_icon} aria-hidden="true" />
                                {/if}
                                {@html heading.text}
                            </a>
                        </li>
                    {/each}
                </ul>
                <div class="">
                    <a
                        href="#/"
                        class="back-to-top text-body-secondary text-small float-end mb-2"
                        on:click={() => window.scrollTo(0, 0)}
                    >
                        <i class="fa-solid fa-arrow-up-to-line" aria-hidden="true" /> Back to top
                    </a>

                    <slot />
                </div>
            </nav>
        </div>
    </div>
{/if}

<style lang="scss">
    @import 'src/styles/_variables.scss';
    nav > ul {
        font-size: 0.875rem;
        list-style: none;
        padding-left: 0;
    }
    .nav {
        overflow-y: auto;
        max-height: calc(100vh - 4rem);
    }

    li {
        border-inline-start: 2pt solid $border-color;
        transition: background-color 0.3s ease-out, border-left 0.3s ease-out;
    }

    li.active {
        a {
            color: $green-800 !important;
        }
        border-left: 2pt solid $success;
        background-color: transparentize($success, 0.75);
    }

    :global([data-bs-theme='dark']) {
        li {
            border-inline-start: 2pt solid $border-color-dark;
        }
        li.active {
            border-left: 2pt solid $success-dark;
            background-color: transparentize($success-dark, 0.35);
            & a {
                color: $gray-200 !important;
            }
        }
    }
</style>
