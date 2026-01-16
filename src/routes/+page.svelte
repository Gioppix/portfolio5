<script lang="ts">
    import { goto } from '$app/navigation';
    import { resolve } from '$app/paths';
    import Window from '$lib/components/Window.svelte';

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

<svelte:window onkeydown={onKeyDown} />

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
    <Window title="Giovanni Feltrin" onclose={close}>
        <div class="window-body">
            <p>Currently building.</p>
        </div>
        <div class="status-bar">
            <p class="status-bar-field">Press F1 for help</p>
            <p class="status-bar-field">Paragraph 1</p>
            <p class="status-bar-field">Grind: 104%</p>
        </div>
    </Window>
    <Window title="Contacts" onclose={close} width="300px" position="bottom-right">
        <div class="window-body">
            <ul class="tree-view">
                <li>
                    <a href="https://www.linkedin.com/in/feltrin-giovanni/" target="_blank">
                        LinkedIn
                    </a>
                </li>
                <li><a href="https://github.com/gioppix/" target="_blank"> GitHub </a></li>
            </ul>
        </div>
    </Window>
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

    .tree-view {
        min-width: 150px;
    }
</style>
