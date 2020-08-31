<script>
  import jq from 'jquery'
  import { createEventDispatcher } from 'svelte'

  export let settings
  let newSettings = {}

  const dispatch = createEventDispatcher()

  $: isValid = (
    newSettings.exerciseTime >= 10 && 
    newSettings.exerciseTime <= 120 &&
    newSettings.restTime >= 0 &&
    newSettings.restTime <= 60 &&
    newSettings.countDownLength >= 0 &&
    newSettings.countDownLength <= 60
  )
  
  $: console.log(settings)

  function save() {
    
  }

  jq(() => {
    jq('#settingsModal').on('show.bs.modal', () => {
      newSettings  = {...settings}
    })
  })
</script>

<div id='settingsModal' class='modal text-dark' tabindex='-1' role='dialog'>
  <div class='modal-dialog' role='document'>
    <div class='modal-content {settings.enableDarkMode ? "bg-dark text-light" : "bg-light text-dark"}'>
      <div class='modal-header'>
        <h4 class='modal-title'>Settings</h4>
        <button type='button' class='btn btn-secondary' id=closeButton data-dismiss='modal' aria-label='Close'>
          <span aria-hidden='true'>&times;</span>
        </button>
      </div>
      <form class='modal-body' id=settingsForm>
        <section class=formSection>
          <h5 class='mb-4'>Time intervals</h5>

          <div class='form-group d-flex align-items-center'>
            <p>Exercises</p>
            <input type=number id=exerciseLengthInput class='form-control ml-auto' min=15 max=120 bind:value={newSettings.exerciseTime} />
            <label for=exerciseLengthInput class='ml-2'> seconds</label>
          </div>

          <div class='form-group d-flex align-items-center'>
            <p>Breaks</p>
            <input type=number id=breakLengthInput class='form-control ml-auto' min=15 max=120 bind:value={newSettings.restTime} />
            <label for=breakLengthInput class='ml-2'> seconds</label>
          </div>

          <div class='form-group d-flex align-items-center'>
            <p>Count down</p>
            <input type=number id=countDownLengthInput class='form-control ml-auto' min=15 max=120 bind:value={newSettings.countDownLength} />
            <label for=countDownLengthInput class='ml-2'> seconds</label>
          </div>
        </section>

        <section class=formSection>
          <h5 class='mb-4'>Theme</h5>
          <div class='form-group d-flex align-items-center'>
            <p>Enable dark theme</p>
            <button
              class='btn ml-auto btn-{newSettings.enableDarkMode ? "primary" : "secondary"}'
              type=button
              on:click={() => {
                newSettings.enableDarkMode = !newSettings.enableDarkMode
              }}
            >
            {newSettings.enableDarkMode ? 'On' : 'Off'}
            </button>
          </div>
        </section>
      </form>
      <div class='modal-footer'>
        <button type='button' class='btn btn-secondary' data-dismiss='modal'>
          Close
        </button>
        <button
          type=submit
          form=settingsForm
          on:click={() => dispatch('save', newSettings)}
          class='btn btn-primary'
          data-dismiss='modal'
          disabled={!isValid}
        >
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

  .formSection:not(:first-child) {
    border-top: 1px solid #dee2e6;
    padding-top: 1rem;
  }

  #closeButton {
    display: flex;
    height: 2rem;
    width: 2rem;
    font-size: 2rem;
    align-items: center;
    justify-content: center;
    border-radius: 50%;
  }
</style>
