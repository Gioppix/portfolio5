<script lang="ts">
    import { resolve } from '$app/paths';
</script>

<div class="bsod">
    <div class="content">
        <p>
            A problem has been detected and windows has been shut down to prevent damage to your
            computer.
        </p>
        <p class="error-code">SESSION3_INITIALIZATION_FAILED</p>
        <p>
            If this is the first time you've seen this stop error screen, restart your computer. If
            this screen appears again, follow these steps:
        </p>
        <p>
            Check to make sure any new hardware or software is properly installed. If this is a new
            installation, ask your hardware or software manufacturer for any windows updates you
            might need.
        </p>
        <p>
            If problems continue, disable or remove any newly installed hardware or software.
            Disable BIOS memory options such as caching or shadowing. If you need to use Safe Mode
            to remove or disable components, restart your computer, press F8 to select Advanced
            Startup Options, and then select Safe Mode.
        </p>
        <p>Technical information:</p>
        <p>*** STOP: 0x0000006F (0xC000003A, 0x00000000, 0x00000000, 0x00000000)</p>
        <a href={resolve('/')}>Click here to try and recover the system.</a>
    </div>
</div>

<style>
    .bsod {
        position: fixed;
        inset: 0;
        background: #0000aa;
        color: #ffffff;
        font-family: 'Lucida Console', 'Courier New', monospace;
        font-size: 16px;
        padding: 10px;
        overflow: auto;
    }

    .content {
        max-width: 900px;
        line-height: 1.5;
    }

    .content p {
        margin: 0 0 1.5em 0;
    }

    .error-code {
        margin-top: 1.5em;
    }

    a {
        color: inherit;
    }
</style>
