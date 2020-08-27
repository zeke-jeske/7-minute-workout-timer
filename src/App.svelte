<script>
import PauseButton from './PauseButton.svelte'
import ResetButton from './ResetButton.svelte'
import SettingsButton from './SettingsButton.svelte'
import SettingsModal from './SettingsModal.svelte'

import { onDestroy } from 'svelte'

let exerciseTime = 30
let restTime = 10
let countDownLength = 10
const interval = setInterval(tick, 1000)
const exercises = [
	  'Jumping jacks',
	  'Wall sit',
	  'Push-ups',
	  'Sit-ups',
	  'Step-ups',
	  'Squats',
	  'Tricep dips',
	  'Plank',
	  'High knees',
	  'Lunges',
	  'Pushups with rotation',
	  'Side plank'
]
	
let timer, exerciseNumber, workoutComplete, resting, workoutStarted, enableDarkMode = true, paused
let bodyClasses = document.body.classList
reset()

$: bodyClasses[enableDarkMode ? 'add' : 'remove']('bg-dark', 'text-light')
$: bodyClasses[!enableDarkMode ? 'add' : 'remove']('bg-light', 'text-dark')
$: if(!workoutStarted) timer = countDownLength
$: upNext = exercises[exerciseNumber + 1]
$: current = workoutStarted
  ? resting ? 'Rest' : exercises[exerciseNumber]
  : 'Get ready...'
	
function tick() {
  if (!paused) {
    // decrease timer
    if (timer > 1) timer--
    // if you're done resting, start the next exercise
    else if (resting) nextExercise()
    // if you're done with the entire the last exercise, finish the workout
    else if (exerciseNumber === exercises.length - 1) finishWorkout()
    // if you're done with an exercise, start resting
    else rest()
  }
}
	
function nextExercise() {
	  workoutStarted = true
	  resting = false
	  timer = exerciseTime
	  exerciseNumber++
}
	
function finishWorkout() {
	  workoutComplete = true
	  clearInterval(interval)
}
	
function rest() {
	  resting = true
	  timer = restTime
}
	
function reset() {
  timer = countDownLength
  exerciseNumber = -1
  workoutComplete = false
  resting = true
  workoutStarted = false
  paused = true
}

function pause() {
  paused = !paused
}

function updateSettings({detail: settings}) {
  exerciseTime = settings.exerciseTime
  restTime = settings.restTime
  countDownLength = settings.countDownLength
  enableDarkMode = settings.enableDarkMode
}

onDestroy(() => clearInterval(interval))
</script>

<style>
  :global(body) {
    padding: 0;
    display: flex;
    justify-content: center;
  }

	* {
		box-sizing: border-box;
		margin: 0;
	}
	
	.container {
		display: flex;
		flex-direction: column;
		align-items: center;
		min-height: 100vh;
		width: 100vw;
		justify-content: center;
		position: relative;
	}
	
	#counter {
		font-size: 10em;
	}
	
	#nextExercise {
		margin: 1rem 0 0;
	}
	
	.invisible {
		visibility: hidden;
	}

</style>

<svelte:window on:keydown={(e) => {
  if (e.key === ' ') pause()
}} />

<SettingsModal settings={{
  exerciseTime,
  restTime,
  countDownLength,
  enableDarkMode
}} on:submit={updateSettings} />

<div class='container text-center'>
  <SettingsButton />
  <ResetButton on:click={reset} />

	{#if (workoutComplete)}
		<h1>Workout complete!</h1>
	{:else}
		<h1 id=counter>{timer}</h1>
		<h1 id=currentExercise>{current}</h1>
		<h2
			id=nextExercise
			class:invisible={!upNext}
		>
			Up next: {upNext}
		</h2>
    <PauseButton paused={paused} on:click={pause} />
  {/if}
</div>
