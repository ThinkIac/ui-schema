<script>
  export let surface;
  export let state = {};
  export let actionUrl = '';

  let form = {};
  let busy = false;
  let error = '';
  let notice = '';

  $: form = { enabled: state.enabled === true, mode: state.mode === 'pseudo' ? 'pseudo' : 'anon' };

  async function save() {
    const action = surface.submitAction || (surface.kind === 'maltese-defaults'
      ? 'maltese.save-defaults' : 'maltese.save-session');
    busy = true;
    error = '';
    notice = '';
    try {
      const response = await fetch(actionUrl, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ action, values: form }),
      });
      const result = await response.json();
      if (!response.ok || result.status >= 400) {
        throw new Error(result.mensagem || result.error || 'Não foi possível salvar.');
      }
      form = result.state || form;
      notice = result.notice || 'Privacidade atualizada.';
    } catch (cause) {
      error = cause.message || 'Não foi possível salvar.';
    } finally {
      busy = false;
    }
  }
</script>

<section class="surface" aria-busy={busy}>
  <header>
    <h3>{surface.title}</h3>
    <p>{surface.description}</p>
  </header>
  <form on:submit|preventDefault={save}>
    <label class="toggle">
      <span>{surface.kind === 'maltese-defaults' ? 'Ativar por padrão' : 'Ativar nesta sessão'}</span>
      <input type="checkbox" bind:checked={form.enabled} disabled={busy} />
    </label>
    <label>
      <span>Nível de ofuscação</span>
      <select bind:value={form.mode} disabled={busy}>
        <option value="anon">Anonimização com marcadores</option>
        <option value="pseudo">Pseudonimização com valores plausíveis</option>
      </select>
    </label>
    <footer><button disabled={busy}>{busy ? 'Salvando…' : 'Salvar'}</button></footer>
  </form>
  {#if error}<p role="alert">{error}</p>{/if}
  {#if notice}<p role="status">{notice}</p>{/if}
</section>

<style>
  .surface, form { display: grid; gap: .85rem; min-width: 0; }
  .surface { max-width: 680px; }
  h3, p { margin: 0; }
  p { color: var(--tk-muted); font-size: .875rem; line-height: 1.45; }
  label { display: grid; gap: .4rem; min-width: 0; }
  .toggle { display: flex; align-items: center; justify-content: space-between; gap: 1rem; }
  .toggle input { width: 1.15rem; height: 1.15rem; accent-color: var(--tk-accent); }
  select, button {
    box-sizing: border-box;
    min-height: 2.5rem;
    padding: .55rem .7rem;
    border: 1px solid var(--tk-border);
    border-radius: var(--tk-radius-md);
    font: inherit;
  }
  select { width: 100%; background: var(--tk-surface); color: var(--tk-text); }
  footer { display: flex; justify-content: flex-end; }
  button { background: var(--tk-accent); color: var(--tk-on-accent); cursor: pointer; }
  button:disabled { opacity: .65; cursor: wait; }
  @media (max-width: 540px) { button { width: 100%; } }
</style>
