<script>
  import Question from "./Question.svelte";
  import { fade } from "svelte/transition";
  let activeQuestion = 0;
  let quiz = getQuizData();
  let score = 0;

  async function getQuizData() {
    const res = await fetch(
      "https://opentdb.com/api.php?amount=10&category=21&type=multiple"
    );
    const data = await res.json();
    console.log(data);
    return data;
  }

  function nextQuestion() {
    activeQuestion += 1;
  }

  function incrementScore() {
    score += 1;
  }

  function resetQuiz() {
    score = 0;
    activeQuestion = 0;
    quiz = getQuizData();
  }
</script>

<style>
  .fade-wrapper {
    position: absolute;
  }
</style>

<div>
  <h4>Score: {score}</h4>
  <h4>Question: {activeQuestion + 1}</h4>
  <button on:click={resetQuiz}>Start New Quiz</button>
  {#await quiz}
    <p>Loading</p>
  {:then data}
    {#each data.results as question, index}
      {#if index === activeQuestion}
        <div transition:fade class="fade-wrapper">
          <Question {question} {nextQuestion} {incrementScore} {resetQuiz} />
        </div>
      {/if}
    {/each}
  {/await}
</div>
