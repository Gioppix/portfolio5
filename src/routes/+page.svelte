<script lang="ts">
    import { onMount } from 'svelte';
    import { goto } from '$app/navigation';
    import { resolve } from '$app/paths';

    let x = 0;
    let y = 0;
    let dragging = false;
    let offsetX = 0;
    let offsetY = 0;
    let container: HTMLDivElement;
    let windowEl: HTMLDivElement;
    let minimized = false;
    let ready = false;

    onMount(() => {
        x = (container.clientWidth - windowEl.offsetWidth) / 2;
        y = (container.clientHeight - windowEl.offsetHeight) / 2;
        ready = true;
    });

    function minimize() {
        minimized = true;
        y = container.clientHeight - windowEl.querySelector('.title-bar')!.offsetHeight;
    }

    function maximize() {
        minimized = false;
        x = (container.clientWidth - windowEl.offsetWidth) / 2;
        y = (container.clientHeight - windowEl.offsetHeight) / 2;
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

    function close() {
        goto(resolve('/bsod'));
    }

    function onKeyDown(e: KeyboardEvent) {
        if (e.key === 'F1') {
            e.preventDefault();
            close();
        }
    }
</script>

<svelte:window
    onmousemove={onMove}
    onmouseup={stopDrag}
    ontouchmove={onMove}
    ontouchend={stopDrag}
    onkeydown={onKeyDown}
/>

<div style="height: 100%; width: 100%; overflow: hidden; position: relative;">
    <div class="sidebar">
        <ul class="tree-view">
            <li>Giovanni Feltrin</li>
            <li>
                <details open>
                    <summary>Experience</summary>
                    <ul>
                        <li>Kuba Labs</li>
                        <li>Tesla</li>
                        <li>Virgo</li>
                        <li>Unger Academy</li>
                    </ul>
                </details>
            </li>
            <li>
                <details open>
                    <summary>Education</summary>
                    <ul>
                        <li>MSc @ UniTrento</li>
                        <li>BSc @ UniTrento</li>
                    </ul>
                </details>
            </li>
        </ul>
    </div>
    <div
        bind:this={container}
        style="height: 100%; width: 100%; position: relative; overflow: hidden;"
    >
        <div
            bind:this={windowEl}
            class="window"
            class:minimized
            class:ready
            style="width: 500px; max-width: 80%; top: {y}px; left: {x}px; position: absolute;"
        >
            <!-- svelte-ignore a11y_no_static_element_interactions -->
            <div
                class="title-bar"
                onmousedown={startDrag}
                ontouchstart={startDrag}
                style="cursor: grab;"
            >
                <div class="title-bar-text">Giovanni Feltrin</div>
                <div class="title-bar-controls">
                    <button aria-label="Minimize" onclick={minimize}></button>
                    <button aria-label="Maximize" onclick={maximize}></button>
                    <button aria-label="Close" onclick={close}></button>
                </div>
            </div>
            <div class="window-body">
                <p>Currently building.</p>
            </div>
            <div class="status-bar">
                <p class="status-bar-field">Press F1 for help</p>
                <p class="status-bar-field">Paragraph 1</p>
                <p class="status-bar-field">Grind: 104%</p>
            </div>
        </div>
    </div>
</div>

<style>
    .sidebar {
        position: absolute;
        top: 0;
        left: 0;
        z-index: 10;
        padding: 10px;
        height: 100%;
        background: inherit;
    }

    .window {
        visibility: hidden;
    }

    .window.ready {
        visibility: visible;
    }

    .window.minimized .window-body,
    .window.minimized .status-bar {
        display: none;
    }

    .tree-view {
        min-width: 150px;
    }
</style>
