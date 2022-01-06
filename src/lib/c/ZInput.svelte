<script>
    export let nameAttr;
    export let nameLabel;
    export let bindValue;
    export let errorText;
    export let handleChange;
</script>
<div>
    <label for={nameAttr}>{nameLabel}</label>
    <input
        placeholder={nameLabel}
        name={nameAttr}
        on:change={handleChange}
        on:blur={handleChange}
        bind:value={bindValue}
    />
    {#if errorText}
        <div class="error-text">{errorText}</div>
    {/if}
</div>
