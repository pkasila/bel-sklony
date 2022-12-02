<script lang="ts">
    let word = '';
    let type = 'asab';

    let slounik = false;

    let outBase = '';
    let outEnding = '';

    const process = () => {
        outBase = '';
        outEnding = '';
        slounik = false;

        if (word.length == 0) {
            alert(`Памылка: увядзіце слова`);
            return;
        }

        const matched = word.toLowerCase().match(/(.*([бвгджзйклмнпрстўфхцчш']))([аеёіуыэюяь]?)$/);

        if (matched == null) {
            alert(`Памылка: увядзіце слова кірыліцай`);
            return;
        }

        const base = matched[1];
        const baseEnd = matched[2];
        const ending = matched[3];

        if (baseEnd.length == 0) {
            alert(`Памылка: аснова "${base}"`);
            return;
        }

        outBase = base;

        switch (baseEnd) {
            case 'ж':
            case 'ш':
            case 'ч':
            case 'ц':
            case 'р':
                // зацвярдзелыя
                if (type === 'asab') {
                    outEnding = 'у';
                } else {
                    outEnding = 'ы';
                }
                break;
            case 'г':
            case 'к':
            case 'х':
                // гкх
                if (type === 'asab') {
                    outEnding = 'у';
                } else {
                    outEnding = 'у/е';
                    slounik = true;
                }
                break;
            default:
                // астатнія: скончваецца на мяккі ці цверды
                if (ending.length > 0 && 'еёіюяь'.includes(ending)) {
                    // мяккі
                    if (type === 'asab') {
                        outEnding = 'ю';
                    } else {
                        outEnding = 'і';
                    }
                } else {
                    // цверды
                    outEnding = 'е';
                }
                break;
        }
    }
</script>

<input bind:value={word} placeholder="Слова"/>

<select bind:value={type}>
    <option value="asab">Асабовы</option>
    <option value="neasab">Неасабовы</option>
</select>

{#if outEnding.length > 0}
    <p>
        Канчатак: <b>{outEnding}</b>

        ({#if slounik}варыятыўнасць, {/if}<a href={'https://slounik.org/search?dict=&search='+encodeURIComponent(word)} target="_blank" rel="noreferrer">Slounik.org</a>)
    </p>
{/if}

<button on:click={process}>
    Які ж канчатак?
</button>
