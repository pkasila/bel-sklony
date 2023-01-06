<script lang="ts">
    const glsn = "аоуыэіеёюя";
    const hard = "шжчрц";

    let word = '';
    let genus = 'male';
    let type = 'usual';
    let common = 'female';
    let person = '1';

    let output: string = null;

    function process() {
        switch (word.toLowerCase()) {
            case 'імя':
                output = 'імі (імені)'
                return;
            case 'племя':
                output = 'племі (племені)'
                return;
            case 'стрэмя':
                output = 'стрэмі (стрэмені)'
                return;
            case 'маці':
                output = 'маці'
                return;
            case 'сірата':
                output = 'сіраце'
                return;
            default:
                break;
        }

        let base = word;
        let lastLetter;

        switch (type) {
            case 'multiple':
                lastLetter = base.charAt(base.length - 1);
                base = base.substring(0, base.length - 1);
                if ((base.charAt(base.length - 1) == 'н') && (base.charAt(base.length - 1) == 'і')) {
                    output = base + "(ях)";
                } else {
                    output = base + "(ах)";
                }
                break;
            case 'substitute':
                lastLetter = base.charAt(base.length - 1);
                if (lastLetter == 'я') {
                    output = base.substring(0, base.length - 2) + "(" + base.charAt(base.length - 2) + "й)";
                } else {
                    if (glsn.includes(base.charAt(base.length - 2))) {
                        if (base.charAt(base.length - 2) == 'я' ||
                            'кгх'.includes(base.charAt(base.length - 3))) {
                            output = base.substring(0, base.length - 2) + "(ім)";
                        } else {
                            output = base.substring(0, base.length - 2) + "(ым)";
                        }
                    } else {
                        output = base.substring(0, base.length - 1) + "(" + base.charAt(base.length - 1) + "м)";
                    }
                }
                break;
            default:
                if (genus === 'none') {
                    // 2 skl
                    while (glsn.includes(base.charAt(base.length - 1))) {
                        base = base.substring(0, base.length - 1);
                    }
                    output = base;
                    switch(output.charAt(output.length -1)) {
                        case 'й':
                        case 'ь':
                            output = output.substring(0, output.length-1);
                            break;
                        case 'т':
                            output = output.substring(0, output.length-1) + 'ц';
                    }
                    output += '(е)';
                } else if (genus === 'male') {
                    // 2 skl
                    output = base;
                    lastLetter = base.charAt(base.length-1);
                    switch(lastLetter) {
                        case 'к':
                            output += '(у)';
                            break;
                        case 'г':
                            output += '(у) / з(е)';
                            break;
                        case 'х':
                            output += '(у) / с(е)';
                            break;
                        default:
                            const asabovy = person === '1';
                            if (lastLetter === 'ь' || lastLetter === 'й') {
                                output = base.substring(0, base.length-1) + (asabovy ? '(ю)' : '(і)')
                            } else if (hard.includes(lastLetter)) {
                                output += asabovy ? '(у)' : '(ы)'
                            } else {
                                switch(output.charAt(output.length -1)) {
                                    case 'й':
                                    case 'ь':
                                        output = output.substring(0, output.length-1);
                                        break;
                                    case 'т':
                                        output = output.substring(0, output.length-1) + 'ц';
                                        break;
                                }
                                output += '(е)';
                            }
                    }
                } else if (genus === 'female' || (genus === 'common' && common === 'female')) {
                    if (!glsn.includes(base.charAt(base.length-1))) { // 3 skl
                        output = base;
                        lastLetter = base.charAt(base.length-1);
                        if (hard.includes(lastLetter)) {
                            output += '(ы)';
                        } else {
                            switch(output.charAt(output.length -1)) {
                                case 'й':
                                case 'ь':
                                    output = output.substring(0, output.length-1);
                                    break;
                                case 'т':
                                    output = output.substring(0, output.length-1) + 'ц';
                                    break;
                            }
                            output += '(і)';
                        }
                    } else { // 1 skl
                        const soft = base.charAt(base.length-1) === 'я';
                        base = base.substring(0, base.length-1);
                        output = base;
                        lastLetter = base.charAt(base.length-1);

                        if (hard.includes(lastLetter)) {
                            output += '(ы)';
                        } else if (soft) {
                            output += '(і)';
                        } else if ('гкх'.includes(lastLetter)) {
                            switch(lastLetter) {
                                case 'к':
                                    output += 'ц(ы) або ц(Э́)';
                                    break;
                                case 'г':
                                    output += 'з(е)';
                                    break;
                                case 'х':
                                    output += 'с(е)';
                                    break;
                            }
                        } else {
                            switch(output.charAt(output.length -1)) {
                                case 'й':
                                case 'ь':
                                    output = output.substring(0, output.length-1);
                                    break;
                                case 'т':
                                    output = output.substring(0, output.length-1) + 'ц';
                                    break;
                            }
                            output += '(е)';
                        }
                    }
                } else if (common === 'male') {
                    lastLetter = base.charAt(base.length - 1);
                    if (lastLetter == 'я') {
                        output = base.substring(0, base.length - 1) + "(ю) / " +
                            base.substring(0, base.length - 1) + "(і́)";
                    } else {
                        output = base.substring(0, base.length - 1) + "(у) / " +
                            base.substring(0, base.length - 1) + "(е́)";
                    }
                } else if (common === 'minor') {
                    while (glsn.includes(base.charAt(base.length - 1))) {
                        base = base.substring(0, base.length - 1);
                    }
                    output = base + "яц(і)";
                }
                break;
        }
    }
</script>

<input bind:value={word} placeholder="Слова"/>

<select bind:value={type}>
    <option disabled>тып</option>
    <option value="usual">звычайны</option>
    <option value="multiple">множналікавы</option>
    <option value="substitute">субстантываваны</option>
</select>
<select bind:value={person}>
    <option disabled>асабовы?</option>
    <option value="1">асабовы</option>
    <option value="0">неасабовы</option>
</select>
<select bind:value={genus}>
    <option disabled>род</option>
    <option value="male">мужчынскі</option>
    <option value="female">жаночы</option>
    <option value="none">ніякі</option>
    <option value="common">агульны</option>
</select>
{#if genus === 'common'}
    <select bind:value={common}>
        <option disabled>агульны які?</option>
        <option value="female">жанчына</option>
        <option value="male">мужчына</option>
        <option value="minor">маладая істота</option>
    </select>
{/if}

{#if output != null}
    <p>
        Вынік: <b>{output}</b>

        (<a href={'https://slounik.org/search?dict=&search='+encodeURIComponent(word)} target="_blank" rel="noreferrer">Slounik.org</a>)
    </p>
{/if}

<button on:click={process}>
    Які ж канчатак?
</button>
