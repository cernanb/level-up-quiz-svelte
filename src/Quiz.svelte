<script>
  import Question from "./Question.svelte";
  import { fade, blur, fly, slide, scale } from "svelte/transition";
  import Modal from "./Modal.svelte";
  import { onMount, beforeUpdate, afterUpdate } from "svelte";
  let activeQuestion = 0;
  let quiz = getQuizData();
  let score = 0;
  let isModalOpen = false;

  onMount(() => {
    console.log("component mounted");
  });

  beforeUpdate(() => {
    console.log("the component will update");
  });
  afterUpdate(() => {
    console.log("i updated");
  });

  async function getQuizData() {
    const res = await fetch(
      "https://opentdb.com/api.php?amount=10&category=21&type=multiple"
    );
    const data = await res.json();
    return data;
  }

  function nextQuestion() {
    activeQuestion += 1;
  }

  function incrementScore() {
    score += 1;
  }

  function resetQuiz() {
    isModalOpen = false;
    score = 0;
    activeQuestion = 0;
    quiz = getQuizData();
  }

  $: if (score > 1) {
    isModalOpen = true;
  }

  $: questionNumber = activeQuestion + 1;
</script>

<style>
  .fade-wrapper {
    position: absolute;
  }
</style>

<div>
  <h4>Score: {score}</h4>
  <h4>Question: {questionNumber}</h4>
  <button on:click|once={resetQuiz}>Start New Quiz</button>
  {#await quiz}
    <p>Loading</p>
  {:then data}
    {#each data.results as question, index}
      {#if index === activeQuestion}
        <div in:fly={{ y: 200 }} out:fly={{ x: -100 }} class="fade-wrapper">
          <Question {question} {nextQuestion} {incrementScore} {resetQuiz} />
        </div>
      {/if}
    {/each}
  {/await}
</div>

{#if isModalOpen}
  <Modal>
    <h2>You won!</h2>
    <p>Congratulations</p>
    <button on:click={resetQuiz}>Start Over</button>
  </Modal>
{/if}
