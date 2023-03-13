<script>
  import { onMount } from "svelte";

  import {
    Col,
    Container,
    Row,
    Input,
    Label,
    FormGroup,
    Button,
  } from "sveltestrap";

  let voices = [];
  let rate = 1;
  let pitch = 1;
  let volume = 1;
  let text = "Hello Emmanuel!";
  let selectedVoice;

  onMount(() => {
    speechSynthesis.onvoiceschanged = () => {
      voices = speechSynthesis.getVoices();
	  selectedVoice = voices [0];
    };
  });

  function printVoice(voice) {
    if (!voice) {
      return "";
    }
    return `${voice.name} (${voice.lang})`;
  }

  function play(){
	const utterance = new SpeechSynthesisUtterance(text);
	speechSynthesis.cancel();
	utterance.rate = rate;
	utterance.pitch = pitch;
	utterance.voice = selectedVoice;
	utterance.volume = volume;
	speechSynthesis.speak(utterance);
  }
</script>
<div class="main">
<h1 style="text-align:center; margin-top:0.5rem">
	<span style="font-weight:700">DOSU</span>  - Speech
</h1>
<p style="text-align:center;">[Text-to-speech Web App]</p>
<ul style="font-size:1rem; color:green">
	<li>
		Type or paste the text: Type or paste the text you want to synthesize into the app's text input field.

	</li>
	<li>
Adjust the voice settings: Pitch, rate, and volume.

	</li>
</ul>
<hr>
<div class="formInput">

<Container>
  <Row>
    <Col>
      <FormGroup>
        <Label for="words">Type something</Label>
        <Input type="text" bind:value={text} id="words" />
      </FormGroup>
    </Col>
  </Row>
  <Row>
    <Col>
      <FormGroup>
        <Label for="voices">Select a voice</Label>
        <Input type="select" bind:value={selectedVoice} id="voices">
          {#each voices as voice}
            <option value={voice}>{printVoice(voice)}</option>
          {/each}
        </Input>
      </FormGroup>
    </Col>
  </Row>
  <Row>
    <Col>
      <FormGroup>
        <Label for="pitch">Pitch</Label>
        <Input
          type="range"
          bind:value={pitch}
          min="0.1"
          max="2"
          step="0.1"
          id="picth"
        />
      </FormGroup>
    </Col>
  </Row>

  <Row>
    <Col>
      <FormGroup>
        <Label for="rate">Rate</Label>
        <Input
          type="range"
          bind:value={rate}
          min="0.1"
          max="2"
          step="0.1"
          id="rate"
        />
      </FormGroup>
    </Col>
  </Row>

  <Row>
    <Col>
      <FormGroup>
        <Label for="volume">Volume</Label>
        <Input
          type="range"
          bind:value={volume}
          min="0.1"
          max="1"
          step="0.1"
          id="volume"
        />
      </FormGroup>
    </Col>
  </Row>
  <Row>
    <Col>
      <FormGroup>
        <Button on:click={play} color="primary">Play</Button>
      </FormGroup>
    </Col>
  </Row>
  <Row>
    <Col>
        Pitch: {pitch} | Speed: {rate} | Volume: {volume} | Voice: {printVoice(selectedVoice)}  
	</Col>
  </Row>
</Container>
</div>
</div>

<footer style="text-align: center; margin:3rem;   padding:0.4rem">Made with <a href="https://svelte.dev/">Svelte.js</a> <img src="https://th.bing.com/th/id/R.02f9ec2d33cc2727b182b07e53a35773?rik=sB8nh4ElbxLn7g&pid=ImgRaw&r=0" alt="">  by <a href="https://emmanueloladosu.com/">Emmanuel Oladosu</a> </footer>
<style>
	a{
		text-decoration: none;
		color: #ff3e00;
	}
	a:hover{
		font-size: 1.2rem;
		font-weight: 500;
	}
	img{
		width: 2rem;
	}
	.main{
		background-color: white;
		margin: 1.2rem;
		border-radius: 3rem;
		padding: 1rem;
	}
	.formInput{
		background-color: rgb(34, 30, 30);
		color: white;
		padding: 1rem;
		border-radius: 2rem;
	}
</style>