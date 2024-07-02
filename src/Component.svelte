<script>
  import { getContext } from "svelte";
  import SuperPopover from "../../bb_super_components_shared/src/lib/SuperPopover/SuperPopover.svelte";
  const { styleable, Block, BlockComponent } = getContext("sdk");
  const component = getContext("component");

  export let text;
  export let icon = "ri-more-fill";

  export let menuItems;
  export let disabled;

  let anchor;
  let openMenu;

  const handleMenu = () => {
    if (disabled) return;
    openMenu = !openMenu;
  };
</script>

<Block>
  <div use:styleable={$component.styles}>
    <button
      class="spectrum-ActionButton spectrum-ActionButton--sizeM spectrum-ActionButton--quiet"
      class:is-selected={openMenu}
      class:is-disabled={disabled}
      bind:this={anchor}
      on:click={handleMenu}
    >
      <i class={icon} />

      {#if text}
        <span class="spectrum-ActionButton-label">{text}</span>
      {/if}
    </button>

    <!-- svelte-ignore missing-declaration -->
    <!-- svelte-ignore a11y-click-events-have-key-events -->
    <SuperPopover open={openMenu} {anchor} on:close={() => (openMenu = false)}>
      <div class="actionMenu" on:click={handleMenu}>
        {#if menuItems?.length}
          {#each menuItems as { text, icon, disabled, onClick }}
            <BlockComponent
              type="plugin/bb-component-SuperButton"
              props={{
                size: "M",
                icon,
                text,
                quiet: true,
                disabled,
                onClick,
              }}
            ></BlockComponent>
          {/each}
        {:else}
          Define Some Menu Action Items
        {/if}
      </div>
    </SuperPopover>
  </div>
</Block>
