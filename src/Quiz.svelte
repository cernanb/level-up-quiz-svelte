<script>
  import Question from "./Question.svelte";
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

  function handleClick() {
    quiz = getQuizData();
  }

  function nextQuestion() {
    activeQuestion += 1;
  }

  function incrementScore() {
    score += 1;
  }
</script>

<div>
  <h4>Score: {score}</h4>
  <h4>Question: {activeQuestion + 1}</h4>
  <button on:click={handleClick}>Start New Quiz</button>
  {#await quiz}
    <p>Loading</p>
  {:then data}
    {#each data.results as question, index}
      {#if index === activeQuestion}
        <Question {question} {nextQuestion} {incrementScore} />
      {/if}
    {/each}
  {/await}
</div>

<!-- {"response_code":0,"results":[{"category":"Sports","type":"multiple","difficulty":"medium","question":"With which team did Michael Schumacher make his Formula One debut at the 1991 Belgian Grand Prix?","correct_answer":"Jordan","incorrect_answers":["Benetton","Ferrari","Mercedes"]},{"category":"Sports","type":"multiple","difficulty":"medium","question":"A stimpmeter measures the speed of a ball over what surface?","correct_answer":"Golf Putting Green","incorrect_answers":[" Football Pitch","Cricket Outfield","Pinball Table"]},{"category":"Sports","type":"multiple","difficulty":"medium","question":"How many scoring zones are there on a conventional dart board?","correct_answer":"82","incorrect_answers":["62","42","102"]},{"category":"Sports","type":"multiple","difficulty":"easy","question":"Which country is hosting the 2018 FIFA World Cup?","correct_answer":"Russia","incorrect_answers":["Germany","United States","Saudi Arabia"]},{"category":"Sports","type":"multiple","difficulty":"medium","question":"Which team was the 2015-2016 NBA Champions?","correct_answer":"Cleveland Cavaliers","incorrect_answers":["Golden State Warriors","Toronto Raptors","Oklahoma City Thunders"]},{"category":"Sports","type":"multiple","difficulty":"hard","question":"Which male player won the gold medal of table tennis singles in 2016 Olympics Games?","correct_answer":"Ma Long (China)","incorrect_answers":["Zhang Jike (China)","Jun Mizutani (Japan)","Vladimir Samsonov (Belarus)"]},{"category":"Sports","type":"multiple","difficulty":"easy","question":"What year did the New Orleans Saints win the Super Bowl?","correct_answer":"2010","incorrect_answers":["2008","2009","2011"]},{"category":"Sports","type":"multiple","difficulty":"easy","question":"What is the most common type of pitch thrown by pitchers in baseball?","correct_answer":"Fastball","incorrect_answers":["Slowball","Screwball","Palmball"]},{"category":"Sports","type":"multiple","difficulty":"easy","question":"Which African American is in part responsible for integrating  Major League baseball?","correct_answer":"Jackie Robinson","incorrect_answers":["Curt Flood","Roy Campanella","Satchell Paige"]},{"category":"Sports","type":"multiple","difficulty":"medium","question":"Who is Manchester United&#039;s top premier league goal scorer?","correct_answer":"Wayne Rooney","incorrect_answers":["Sir Bobby Charlton","Ryan Giggs","David Beckham"]}]} -->
