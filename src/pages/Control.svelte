<script>
    import { sendMessage, subscribe } from "../utils/messages";

    let heatingMode = "manual";
    let heatingLevel = 67;
    let temperature = 21;
    let checkbox = false;
    let window = false;

    let currentTemperature = 21;
    let currentHumidity = 75;

    subscribe(([type, data]) => {
        if (type === "t") currentTemperature = parseFloat(data);
        if (type === "h") currentHumidity = parseFloat(data);
        if (type === "o") window = data === "1";
        console.log(type, data);
    });

    $: heatingMode = checkbox ? "auto" : "manual";
</script>

<div class="container">
    <div class="section center">
        <div class="row center">
            <!-- Dane -->
            <h2 class="header">Aktualne Dane</h2>
            <p class="flow-text">
                <i class="material-icons">speed</i>
                Temperatura:
                {currentTemperature}°C
            </p>
            <p class="flow-text">
                <i class="material-icons">opacity</i>
                Wilgotność:
                {currentHumidity}%
            </p>
            <p class="flow-text">
                <i class="material-icons">ac_unit</i>
                Status Okna:
                {window ? 'Otwarte' : 'Zamknięte'}
            </p>

            <!-- Sterowanie -->
            <h2 class="header">Sterowanie Grzejnikiem</h2>
            <div class="switch">
                <label class="flow-text">
                    Manualne
                    <input
                        type="checkbox"
                        on:change={() => (checkbox = !checkbox)} />
                    <span class="lever" />
                    Automatyczne
                </label>
            </div>

            {#if heatingMode === 'manual'}
                <div>
                    <p class="flow-text">Ustaw moc grzejnika</p>
                    <span class="flow-text">{heatingLevel}%</span>
                    <p class="range-field col s12">
                        <input
                            type="range"
                            class="col s12 m4 offset-m4"
                            bind:value={heatingLevel}
                            min="0"
                            max="100" />
                    </p>
                    <div class="col s12">
                        <button
                            class="btn waves-effect waves-light "
                            type="submit"
                            name="action"
                            on:click={() => sendMessage('g', heatingLevel)}>
                            Zatwierdź
                            <i class="material-icons right">send</i>
                        </button>
                    </div>
                </div>
            {:else}
                <div>
                    <p class="flow-text">Ustaw temperaturę</p>
                    <span class="flow-text">{temperature}°C</span>
                    <p class="range-field col s12">
                        <input
                            type="range"
                            class="col s12 m4 offset-m4"
                            bind:value={temperature}
                            min="15"
                            max="26"
                            step="0.5" />
                    </p>
                    <div class="col s12">
                        <button
                            class="btn waves-effect waves-light "
                            type="submit"
                            name="action"
                            on:click={() => sendMessage('a', temperature)}>
                            Zatwierdź
                            <i class="material-icons right">send</i>
                        </button>
                    </div>
                </div>
            {/if}
        </div>
    </div>
</div>
