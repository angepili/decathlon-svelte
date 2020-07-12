<div class="autocomplete">
    <input type="text" class="autocomplete-input"
        bind:this="{input}"
        bind:value="{text}"
        onInput={handleInput}
        on:input="{oninput}"
        on:focus="{open}"
        on:keydown="{onkeydown}"
        on:keypress="{onkeypress}"
        placeholder={placeholder}
        >
    <div class="autocomplete-list {opened ? '' : 'hidden'}" bind:this="{list}">
        {#each filteredData as item, i}
            <div class="autocomplete-list-item {i === highlightIndex ? 'selected' : ''}"
                on:click="{() => onclick(item)}">

                {@html item.highlighted && item.highlighted.name || item.name}
            </div>
        {/each}
    </div>
</div>

<svelte:window on:click={onDocumentClick}/>

<script>

export let data = [];
export let value = null;
export let text = '';
export let handleInput;
export let placeholder;
let opened = false;
let highlightIndex = 0;
let input, list, filteredData;

// reactive block
$: {
    const f = text.toLowerCase();
    const hlfilter = highlightFilter(text, ['name']);
    filteredData = !text ? data : data
        .filter(item => item.name.toLowerCase().includes(f))
        .map(hlfilter);
}

function selectItem () {
    value = filteredData[highlightIndex];
    text = value.name;
    close();
}

function up () {
    open();
    if (highlightIndex > 0) highlightIndex--;
    highlight();
}

function down () {
    open();
    if (highlightIndex < filteredData.length - 1) highlightIndex++;
    highlight();
}

function highlight () {
    const el = list.querySelector('.selected');
    if (el) el.scrollIntoViewIfNeeded();
}


function onclick (item) {
    value = item;
    text = item.name;
    close();
}

function onDocumentClick (e) {
    if (!e.target.closest('.autocomplete')) close();
}

function onkeydown (e) {
    let key = e.key;
    if (key === 'Tab' && e.shiftKey) key = 'ShiftTab';
    const fnmap = {
        Tab: opened ? down.bind(this) : null,
        ShiftTab: opened ? up.bind(this) : null,
        ArrowDown: down.bind(this),
        ArrowUp: up.bind(this),
        Escape: onEsc.bind(this),
    };
    const fn = fnmap[key];
    if (typeof fn === 'function') {
        e.preventDefault();
        fn(e);
    }
}

function oninput(e) {
    handleInput(e);
    text = e.target.value;
    open();
    highlightIndex = 0;
}

function onkeypress (e) {
    if (e.key === 'Enter') {
        e.preventDefault();
        selectItem();
    }
}

function onEsc (e) {
    if (text) return clear();
    e.stopPropagation();
    if (opened) {
        input.focus();
        close();
    };
}

function clear () {
    text = '';
    setTimeout(() => input.focus());
}

function open () {
    opened = true;
}

function close () {
    opened = false;
}


// 'item number one'.replace(/(it)(.*)(nu)(.*)(one)/ig, '<b>$1</b>$2 <b>$3</b>$4 <b>$5</b>')
function highlightFilter (q, fields) {
    const qs = '(' + q.trim().replace(/\s/g, ')(.*)(') + ')';
    const reg = new RegExp(qs, 'ig');
    let n = 1, len = qs.split(')(').length + 1, repl = '';
    for (; n < len; n++) repl += n % 2 ? `<b>$${n}</b>` : `$${n}`;

    return i => {
        const newI = Object.assign({ highlighted: {} }, i);
        if (fields) {
            fields.forEach(f => {
                if (!newI[f]) return;
                newI.highlighted[f] = newI[f].replace(reg, repl);
            });
        }
        return newI;
    };
}

</script>

<style>
    .autocomplete {
        min-width: 200px;
        max-width: 100%;
        background: #fff;
        border-radius: 0;
        border: 1px solid #ccc;
        position: relative;
    }
    .autocomplete * { box-sizing: border-box; }
    .autocomplete-input {
        font: inherit;
        width: 100%;
        border: none;
        padding: 5px 11px;
        margin: 10px 0;
        outline: none;
        text-transform: uppercase;
    }
    .autocomplete-list {
        position: absolute;
        left: 0;
        right: 0;
        overflow-y: auto;
        z-index: 99;
        padding: 0;
        border: 1px solid #ccc;
        max-height: calc(10 * (1em + 10px) + 15px);
        user-select: none;
        box-shadow: 0 4px 20px rgba(0,0,0,.25);
        background-color: #fff;
    }
    .autocomplete-list:empty { padding: 0;  }
    .autocomplete-list-item { 
        padding: 12px 20px 12px 40px; 
        color: #333; 
        cursor: pointer; 
        line-height: 1;  
        text-transform: uppercase;
        position: relative;
    }
    .autocomplete-list-item:after {
                content: '';
                position: absolute;
                left: 4px;
                top: 0;
                bottom: 0;
                width: 24px;
                background-image: url(https://svgshare.com/i/MrF.svg);
                background-size: 26px;
                background-repeat: no-repeat;
                background-position: center center;
            }
    .autocomplete-list-item:hover,
    .autocomplete-list-item.selected { background-color: #fcfcfc; }
    .autocomplete-list.hidden { display: none; }
</style>