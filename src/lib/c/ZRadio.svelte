<script>
    export let nameAttr;
    export let nameLabel;
    export let itemList;
    export let itemValueChecked;
    export let errorText;
    export let handleChange;

    function isChecked(checkedValue, itemValue) {
        if(checkedValue === itemValue) {
            return true;
        }
        else {
            return false;
        }
    }
</script>
<div>
    <label for={nameAttr}>{nameLabel}</label>
    {#each itemList as p, i}
        <label class="compact">
            <input
                type="radio"
                id={`${nameAttr}-${p.value}`}
                name={nameAttr}
                value={p.value}
                on:change={handleChange}
                on:blur={handleChange}
                checked={isChecked(itemValueChecked, p.value)}
            />
            <span> {p.label}{#if p.label != p.id}[{p.id}]{/if}</span>
        </label>
    {/each}
    {#if errorText}
        <div class="error-text">{errorText}</div>
    {/if}
</div>
