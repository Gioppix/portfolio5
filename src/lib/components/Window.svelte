<script lang="ts">
    import { onMount, type Snippet } from 'svelte';

    interface Props {
        title: string;
        width?: string;
        onclose?: () => void;
        children: Snippet;
        position?: 'center' | 'bottom-right';
    }

    let { title, width = '500px', onclose, children, position = 'center' }: Props = $props();

    let x = $state(0);
    let y = $state(0);
    let dragging = $state(false);
    let offsetX = 0;
    let offsetY = 0;
    let container: HTMLDivElement;
    let windowEl: HTMLDivElement;
    let titleBar: HTMLDivElement;
    let minimized = $state(false);
    let ready = $state(false);
    let prevX = 0;
    let prevY = 0;

    onMount(() => {
        if (position === 'bottom-right') {
            x = container.clientWidth - windowEl.offsetWidth - 20;
            y = container.clientHeight - windowEl.offsetHeight - 20;
        } else {
            x = (container.clientWidth - windowEl.offsetWidth) / 2;
            y = (container.clientHeight - windowEl.offsetHeight) / 2;
        }
        ready = true;
    });

    function minimize() {
        prevX = x;
        prevY = y;
        minimized = true;
        y = container.clientHeight - titleBar.offsetHeight;
    }

    function maximize() {
        if (minimized) {
            minimized = false;
            x = prevX;
            y = prevY;
        }
    }

    function startDrag(e: MouseEvent | TouchEvent) {
        dragging = true;
        const point = 'touches' in e ? e.touches[0] : e;
        offsetX = point.clientX - x;
        offsetY = point.clientY - y;
    }

    function onMove(e: MouseEvent | TouchEvent) {
        if (dragging) {
            const point = 'touches' in e ? e.touches[0] : e;
            x = point.clientX - offsetX;
            if (!minimized) {
                y = point.clientY - offsetY;
            }
        }
    }

    function stopDrag() {
        dragging = false;
    }
</script>

<svelte:window
    onmousemove={onMove}
    onmouseup={stopDrag}
    ontouchmove={onMove}
    ontouchend={stopDrag}
/>

<div bind:this={container} class="window-container">
    <div
        bind:this={windowEl}
        class="window"
        class:minimized
        class:ready
        style="width: {width}; max-width: 80%; top: {y}px; left: {x}px; position: absolute;"
    >
        <!-- svelte-ignore a11y_no_static_element_interactions -->
        <div
            bind:this={titleBar}
            class="title-bar"
            onmousedown={startDrag}
            ontouchstart={startDrag}
            style="cursor: grab;"
        >
            <div class="title-bar-text">{title}</div>
            <div class="title-bar-controls">
                <button aria-label="Minimize" onclick={minimize}></button>
                <button aria-label="Maximize" onclick={maximize}></button>
                <button aria-label="Close" onclick={onclose}></button>
            </div>
        </div>
        {@render children()}
    </div>
</div>

<style>
    .window-container {
        position: absolute;
        inset: 0;
        overflow: hidden;
        pointer-events: none;
    }

    .window {
        visibility: hidden;
        pointer-events: auto;
    }

    .window.ready {
        visibility: visible;
    }

    .window.minimized :global(.window-body),
    .window.minimized :global(.status-bar) {
        display: none;
    }
</style>
