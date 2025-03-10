<script lang="ts">
  import ChevronDown from '$lib/utils/ChevronDown.svelte';
  import ChevronUp from '$lib/utils/ChevronUp.svelte';
  import classNames from 'classnames';
  import { getContext, onMount } from 'svelte';
  import { writable } from 'svelte/store';
  import { fade, blur, fly, slide } from 'svelte/transition';
  import type { TransitionTypes, TransitionParamTypes } from '../types';
  import type { AccordionCtxType } from './Accordion.svelte';

  export let open: boolean = false;
  export let activeClasses: string | undefined = undefined;
  export let inactiveClasses: string | undefined = undefined;

  export let defaultClass: string =
    'flex items-center justify-between w-full font-medium text-left group-first:rounded-t-xl';

  export let transitionType: TransitionTypes = 'slide';
  export let transitionParams: TransitionParamTypes = {};

  // make a custom transition function that returns the desired transition
  const multiple = (node: HTMLElement, params: any) => {
    switch (transitionType) {
      case 'slide':
        return slide(node, params);
      case 'blur':
        return blur(node, params);
      case 'fly':
        return fly(node, params);
      case 'fade':
        return fade(node, params);
    }
  };

  const ctx = getContext<AccordionCtxType>('ctx') ?? {};

  // single selection
  const self = {};
  const selected = ctx.selected ?? writable();

  let _open: boolean = open;
  open = false;

  onMount(() => {
    if (_open) $selected = self;

    // this will trigger unsubscribe on destroy
    return selected.subscribe((x) => (open = x === self));
  });

  const handleToggle = (e: Event) => selected.set(open ? {} : self);

  let buttonClass: string;
  $: buttonClass = classNames(
    defaultClass,
    ctx.flush ? 'py-5' : 'p-5',
    open && (ctx.flush ? 'text-gray-900 dark:text-white' : activeClasses || ctx.activeClasses),
    !open && (ctx.flush ? 'text-gray-500 dark:text-gray-400' : inactiveClasses || ctx.inactiveClasses),
    $$props.class
  );
</script>

<h2 aria-expanded={open} class="group">
  <button on:click={handleToggle} type="button" class={buttonClass}>
    <slot name="header" />
    {#if open}
      <slot name="arrowup"><ChevronUp /></slot>
    {:else}
      <slot name="arrowdown"><ChevronDown /></slot>
    {/if}
  </button>
</h2>
{#if open}
  <div transition:multiple={transitionParams}>
    <div class={ctx.flush ? 'py-5' : 'p-5'}>
      <slot />
    </div>
  </div>
{/if}
