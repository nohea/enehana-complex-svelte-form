<script>
    import { getScopedObj } from "$lib/util";
    import { getContext } from "svelte";
    import { key } from "svelte-forms-lib";
    export let nameAttr;
    export let nameLabel;
    export let itemList;
    export let itemValueChecked;
    // allows the Form* components to share state with the parent form
    const { form, errors, handleChange } = getContext(key);

    function isChecked(checkedValue, itemValue) {
        return checkedValue === itemValue ? true : false;
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
    {#if getScopedObj($errors, nameAttr)}
        <div class="form-error">{getScopedObj($errors, nameAttr)}</div>
    {/if}
</div>
