<script>
    import { onMount, createEventDispatcher, afterUpdate, onDestroy } from "svelte";
    import { browser } from "$app/environment";
    /**@type {string} -(required) API server host */
    export let host;
    /**@type {string} - (required) The website ID*/
    export let appId;
    /**@type {"light"|"auto"|"dark"}*/
    export let theme;
    /**@type {string} -  Current page URL. Used to display on dashboard.*/
    export let pageUrl;
    /**@type {string} - (required) Current page ID. Used to identity your page. Should be unique in a website. Such as page slug, permalink.*/
    export let pageId;
    /**@type {string} -  Current page title. Used to display on dashboard*/
    export let pageTitle;
    export let lang = undefined;

    const dispatch = createEventDispatcher();

    const load = ()=>{
        dispatch("load");
    }



 
    onDestroy(()=>{
        if (browser){
            let url = "https://cusdis.com/js/cusdis.es.js";
            let script  = document.querySelector(`script[src="${url}"]`);
            script.removeEventListener("load", load);
        }
    })

    afterUpdate(async ()=>{
        load();
    })

    onMount(async ()=>{
        if (lang){
            let url = `https://cusdis.com/js/widget/lang/${lang}.js`;
            let langScript = document.querySelector(`script[src="${url}"]`);
            if (!langScript){
                langScript = document.createElement("script");
                langScript.setAttribute("defer",true);
                langScript.src = url;
                document.querySelector("head").appendChild(langScript);
            }
        }

        let url = "https://cusdis.com/js/cusdis.es.js";
        let script  = document.querySelector(`script[src="${url}"]`);
        if (!script){
            script = document.createElement("script");
            script.src = url;
            script.defer = true;
            script.async = true;
            script.addEventListener("load", load);
            document.querySelector("head").appendChild(script);
        }
        window.CUSDIS.initial();
    })
</script>

<div
id = "cusdis_thread"
data-host = {host || "https://cusdis.com"}
data-app-id = {appId}
data-page-id = {pageId}
data-page-url = {pageUrl}
data-page-title = {pageTitle} 
data-theme = {theme}
/>