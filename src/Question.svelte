<script>
  export let question;
  export let nextQuestion;
  export let incrementScore;
  let result;
  let answered = false;
  let randomAnswers = question.incorrect_answers
    .concat(question.correct_answer)
    .sort(() => Math.random() - 0.5);

  function checkAnswer(answer) {
    answered = true;
    if (answer === question.correct_answer) {
      incrementScore();
      return (result = "Correct!");
    }
    return (result = "Incorrect!");
  }
</script>

<h3>
  {@html question.question}
</h3>

{#each randomAnswers as answer}
  <button
    style="color: {answered ? (answer === question.correct_answer ? 'green' : 'red') : ''};"
    disabled={answered}
    on:click={() => checkAnswer(answer)}>
    {@html answer}
  </button>
{/each}
<br />
{#if answered}
  <button on:click={nextQuestion}>Next Question</button>
{/if}
