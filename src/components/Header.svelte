<script lang='ts'>
    import { data, userLang } from '$lib/data';
    import { browser } from '$app/environment';
    import { page } from '$app/stores';

    let isSticky = false;
    $: if (browser) {
        data.setLang($userLang);
        isSticky = window.scrollY > 0;
        window.addEventListener('scroll', () => {
            isSticky = window.scrollY > 0;
        });
    }
    
    $: currentUrl = $page.url.pathname;
    $: header = $data.header;
</script>

<header>
    <div class="partner-sites" style={isSticky ? 'display: none;' : ''}>
        <ul>
            {#each header.partners as {href, title}}
            <li><a href={href}>{title}</a></li>
            {/each}
        </ul>
    </div>
    <nav>
        <ul>
            <div class="logo">
                {#if isSticky}
                    <a href={header.logos[1].href}><img src={header.logos[1].src} alt="CAPS-CPCA Logo"/></a>
                {:else}
                    <a href={header.logos[0].href} target="_blank"><img src={header.logos[0].src} alt="SOGC Logo"/></a>
                {/if}
            </div>
            {#each header.nav as {type, title, content, href}}
                {#if type === 'normal'}
                <li><a href={href} class:selected={href === currentUrl}>{title}</a></li>
                {:else if type === 'content'}
                    {#each content as {title, href}}
                    <li><a data-sveltekit-noscroll href={href} class:selected={currentUrl.includes(href)}>{title}</a></li>
                    {/each}
                {:else if type === 'exit'}
                <li><a href={href} target="_self"><span id="exit">{title}</span></a></li>
                {:else if type === 'switch'}
                <li><button on:click={data.togLang}>{title}</button></li>
                {/if}
            {/each}
        </ul>
    </nav>
</header>

<style>
/* Specific Sheet for Navigation bar */
    .logo {
        position: absolute;
        top: 0;
        left: 0;
        height: 90px;
        background: #f1f1f1;
        transition: all 0.3s ease-in-out;
    }
    .logo:hover {
        background: #b5b5b5;
    }
    header {
        position: fixed;
        width: 100%;
        top: 0;
        z-index: 1000;
        background: var(--Primary);
    }
    .selected {
        color: var(--Highlight);
    }
    button {
        background: none;
        border: 1px solid var(--Background);
        font-family: 'Roboto', sans-serif;
        font-size: 1rem;
        font-weight: 500;
        color: var(--Background);
        cursor: pointer;
    }
    .partner-sites {
        height: 2.5rem;
        display: flex;
        justify-content: center;
        gap: 1rem;
        background: white;
    }
    .partner-sites ul li:first-of-type {
        display: flex;
        align-items: center;
        padding: 0px 2rem;
        height: 3rem;
        background: var(--Primary);
    }
    .partner-sites ul li:first-of-type a {
        color: var(--Background);
    }
    .partner-sites ul li a{
        font-family: 'Roboto', sans-serif;
        font-size: 0.85rem;
        font-weight: 300;
        text-transform: uppercase;
        color: var(--Primary);
        text-decoration: none;
    }
    .partner-sites ul li a:hover {
        color: var(--Highlight);
    }
    nav {
        height: 4rem;
        display: flex;
        justify-content: center;
        align-items: center;
        background: var(--Primary);
        border-bottom: 1px solid var(--Primary-stroke);
    }
    ul {
        margin: 0;
        padding: 0;
        height: 100%;
        width: 1200px;
        position: relative;
        display: flex;
        align-items: center;
        justify-content: flex-end;
        gap: 2rem;
        list-style-type: none;
    }
    ul li {
        font-family: 'Roboto', sans-serif;
        font-size: 0.95rem;
        font-weight: 400;
        text-transform: uppercase;
    }
    ul li a {
        letter-spacing: 1px;
        font-family: 'Roboto', sans-serif;
        font-size: 1rem;
        font-weight: 500;
        text-decoration: none;
        text-transform: uppercase;
        color: var(--Background);
        transition: all 0.1s ease-in-out;
    }
    ul li a:hover {
        color: var(--Highlight);
    }
    ul li a:focus {
        color: var(--Pressed);
    }
    #exit {
        color: white;
        background: #9E1A30;
        padding: 0px 15px;
        border-radius: 4px;
    }
    #exit:hover {
        color: white;
        background: #EE4B6A;
    }
</style>