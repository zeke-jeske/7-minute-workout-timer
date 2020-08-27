<script>
  import { createEventDispatcher } from 'svelte'

  export let settings

  const dispatch = createEventDispatcher()

  function submitForm() {
    dispatch('submit', settings)
  }
</script>

<div id='settingsModal' class='modal text-dark' tabindex='-1' role='dialog'>
  <div class='modal-dialog' role='document'>
    <div class='modal-content'>
      <div class='modal-header'>
        <h5 class='modal-title'>Settings</h5>
        <button type='button' class='close' data-dismiss='modal' aria-label='Close'>
          <span aria-hidden='true'>&times;</span>
        </button>
      </div>
      <form class='modal-body' id=settingsForm on:submit>
        <div class='form-group'>
          <label>
            Exercise length:
            <input type=number class='form-control' min=15 max=120 bind:value={settings.exerciseTime}/>
            seconds
          </label>
        </div>

        <div class='form-group'>
          <label>
            Break length:
            <input type=number class='form-control' min=3 max=60 bind:value={settings.restTime}/>
            seconds
          </label>
        </div>

        <div class='form-group'>
          <label>
            Count down length:
            <input type=number class='form-control' min=0 max=30 bind:value={settings.countDownLength} /> seconds
          </label>
        </div>

        <div class='form-group'>
          <label>
            Enable dark mode:
            <button
              class='btn btn-{settings.enableDarkMode ? "primary" : "secondary"}'
              type=button
              on:click={() => settings.enableDarkMode = !settings.enableDarkMode}
            >
              {settings.enableDarkMode ? 'On' : 'Off'}
            </button>
          </label>
        </div>
      </form>
      <div class='modal-footer'>
        <button type='button' class='btn btn-secondary' data-dismiss='modal'>
          Close
        </button>
        <button type=submit form=settingsForm on:click={submitForm} class='btn btn-primary' data-dismiss='modal'>
          Save changes
        </button>
      </div>
    </div>
  </div>
</div>

<style>
  .form-control {
    width: auto;
    display: inline;
  }
</style>
