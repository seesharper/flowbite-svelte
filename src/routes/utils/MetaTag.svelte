<script lang="ts">
  import { MetaTags } from 'svelte-meta-tags';
  export let breadcrumb_title: string = '';
  export let description: string = '';
  export let title: string = '';
  export let dir: string = '';
  let dirstring = dir.toLowerCase();
  let breadcrumb = breadcrumb_title.toLowerCase().replaceAll(' ', '-');
</script>

<MetaTags
  title={breadcrumb_title}
  titleTemplate="%s | Flowbite-Svelte"
  {description}
  facebook={{
    appId: '453670756870545'
  }}
  openGraph={{
    type: 'website',
    url: `https://flowbite-svelte.com/${dirstring}/${breadcrumb}`,
    title: `${title}`,
    description: `${description}`,
    images: [
      {
        url: `https://open-graph-vercel.vercel.app/api/flowbite-svelte?title=${breadcrumb_title}`,
        width: 1200,
        height: 630,
        alt: `${title}`
      }
    ],
    site_name: 'Flowbite-Svelte'
  }}
  twitter={{
    handle: '@shinokada',
    cardType: 'summary_large_image',
    title: `${title}`,
    description: `${description}`,
    image: `https://open-graph-vercel.vercel.app/api/flowbite-svelte?title=${breadcrumb_title}`,
    imageAlt: `${title}`
  }} />
