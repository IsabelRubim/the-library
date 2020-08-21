<script>
  import { navigate } from 'svelte-routing';

  import BackButtonRow from '../common/BackButtonRow.svelte';
  import BookCover from '../common/BookCover.svelte';
  import Button from '../common/Button.svelte';
  import Header from '../common/Header.svelte';
  import { httpPost } from '../common/api';
  import TextInput from './TextInput.svelte';

  let title = '';
  let author = '';
  let cover = '';
  let about = '';

  $: book = { title, author, cover, about };

  async function handleSubmit(event) {

    function getRandomInt(min, max) {
      min = Math.ceil(min);
      max = Math.floor(max);
      return Math.floor(Math.random() * (max - min + 1)) + min;
    }

    const newBook = {
      ...book,
      variation: getRandomInt(0, 2),
      favorite: false,
    }

    const { ok } = await httpPost('/', newBook);

    if (ok) {
      navigate('/');
    }
  }

</script>

<style>

  form {
    display: grid;
    grid-auto-rows: auto;
    grid-template-columns: 1fr;
    gap: var(--typeSizeXXLarge);
  }

  .fields {
    display: grid;
    grid-auto-rows: auto;
    gap: var(--spacingMedium);
  }

  .preview {
    display: grid;
    grid-template-columns: minmax(20vw, 10rem);
    grid-template-rows: minmax(32vw, 16rem);
  }

  @media (min-width: 48rem) {
    form {
      grid-template-columns: 60vw 20vw;
      gap: var(--typeSizeXLarge);
    }
  }

</style>

<BackButtonRow />

<Header element="h1" size="large">Create</Header>

<form on:submit|preventDefault={handleSubmit} >
  <div class="fields">
    <TextInput label="Title" bind:value={title} />
    <TextInput label="Author" bind:value={author} />
    <TextInput label="Cover URL" bind:value={cover} />
    <TextInput label="About" bind:value={about} multiline />
    <div>
      <Button>Save</Button>
    </div>
  </div>

  <div>
    <Header>Preview</Header>
    <div class="preview">
      <BookCover {book} />
    </div>
  </div>
</form>