<script lang="ts">
    const glsn = "аоуыэіеёюя";
    const miag = "іеёюя";
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

        function run1Skl() {
            const soft = base.charAt(base.length - 1) === 'я';
            base = base.substring(0, base.length - 1);
            output = base;
            lastLetter = base.charAt(base.length - 1);

            if (hard.includes(lastLetter)) {
                output += '(ы)';
            } else if (soft) {
                output += '(і)';
            } else if ('гкх'.includes(lastLetter)) {
                output = output.substring(0, output.length - 1);
                switch (lastLetter) {
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
                switch (output.charAt(output.length - 1)) {
                    case 'й':
                    case 'ь':
                        output = output.substring(0, output.length - 1);
                        break;
                    case 'т':
                        output = output.substring(0, output.length - 1) + 'ц';
                        break;
                }
                output += '(е)';
            }
        }

        function run2Skl() {
            const asabovy = person === '1';
            output = base;
            lastLetter = base.charAt(base.length - 1);
            let g;
            while (glsn.includes(base.charAt(base.length - 1))) {
                g = base.charAt(base.length - 1);
                base = base.substring(0, base.length - 1);
            }
            switch (lastLetter) {
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
                    if (miag.includes(g)) {
                        output = base + (asabovy ? '(ю)' : '(і)');
                    } else if (lastLetter === 'ь' || lastLetter === 'й') {
                        base = base.substring(0, base.length - 1);
                        if (base.match(/(.*)([ао]н)$/) !== null) {
                            base = base.substring(0, base.length - 2) + 'н';
                        }
                        output = base + (asabovy ? '(ю)' : '(і)');
                    } else if (hard.includes(lastLetter)) {
                        output += asabovy ? '(у)' : '(ы)'
                    } else {
                        output = base;
                        switch (output.charAt(output.length - 1)) {
                            case 'й':
                            case 'ь':
                                output = output.substring(0, output.length - 1);
                                break;
                            case 'т':
                                output = output.substring(0, output.length - 1) + 'ц';
                                break;
                            case 'д':
                                output = output.substring(0, output.length - 1) + 'дз';
                                break;
                        }
                        output += '(е)';
                    }
                    break;
            }
        }

        function run3Skl() {
            output = base;
            lastLetter = base.charAt(base.length - 1);
            if (hard.includes(lastLetter)) {
                output += '(ы)';
            } else {
                switch (output.charAt(output.length - 1)) {
                    case 'й':
                    case 'ь':
                        output = output.substring(0, output.length - 1);
                        break;
                    case 'т':
                        output = output.substring(0, output.length - 1) + 'ц';
                        break;
                }
                output += '(і)';
            }
        }

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
                    run2Skl();
                } else if (genus === 'male') {
                    if ('ая'.includes(base.charAt(base.length - 1))) {
                        output = base.substring(0, base.length - 1);
                        output += base.charAt(base.length - 1) === 'я' ? '(ю)' : '(у)';
                    } else {
                        // 2 skl
                        run2Skl();
                    }
                } else if (genus === 'female' || (genus === 'common' && common === 'female')) {
                    if (!glsn.includes(base.charAt(base.length - 1))) { // 3 skl
                        run3Skl();
                    } else { // 1 skl
                        run1Skl();
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
<select bind:value={genus}>
    <option disabled>род</option>
    <option value="male">мужчынскі</option>
    <option value="female">жаночы</option>
    <option value="none">ніякі</option>
    <option value="common">агульны</option>
</select>
{#if genus === 'male'}
    <select bind:value={person}>
        <option disabled>асабовы?</option>
        <option value="1">асабовы</option>
        <option value="0">неасабовы</option>
    </select>
{/if}
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
