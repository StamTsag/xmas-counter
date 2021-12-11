<script context='module'>
    export const ssr = false;
</script>

<script>
    import { onMount } from 'svelte';
    import Particles from 'svelte-particles';

    // initialise dates
    const currentDate = new Date();
    var xmasDate = new Date(currentDate.getFullYear(), 11, 25);

    // if date exceeds this year's xmas, increase year
    if(currentDate.getMonth() == 11 && currentDate.getDate() > 25) {
        xmasDate.setFullYear(xmasDate.getFullYear() + 1); 
    }

    // one day = 1000 * (seconds * minutes * hours) to convert to ms
    const oneDay = 1000 * (60 * 60 * 24);

    // get the nearest integer of the deduction of the xmas date from the current one divided by one day, in ms
    let daysLeft = Math.ceil((xmasDate.getTime() - currentDate.getTime()) / oneDay);

    let particleOptions = {
        particles: {
            color: {
                value: '#ffffff'
            },

            move: {
                enable: true,
                direction: 'bottom',
                speed: 1.5
            },

            number: {
                density: {
                    enable: true,
                    area: 500
                },
                // kind of useless overriden
                value: 30
            },
            
            opacity: {
                value: .8
            },

            shape: {
                type: 'circle'
            },

            size: {
                value: 10
            },

            // wobbly wobbly right left
            wobble: {
                enable: true,
                distance: 20,
                speed: 1
            },
            
            // size randomness
            zIndex: {
                value: { min: 0, max: 150 }
            }
        }
    };

    if(daysLeft == 0) {
        particleOptions.particles.number.value = 60;

    } else {
        // dynamic properties dependant on days left
        // formula: minimumSnowAmount + abs(daysLeft - maxYearDays) * ratioMultiplier
        const snowToOutput = 1 + Math.abs((daysLeft - 366) * 0.1);

        particleOptions.particles.number.value = snowToOutput;
    }

    onMount(() => {
        // get days element
        const xmasDays = document.getElementById('xmas-days');

        if(daysLeft == 0) {
            xmasDays.textContent = 'Merry Christmas!';
            
        } else {
            // adjust if last day
            daysLeft > 1 ? xmasDays.textContent = daysLeft + ' days till Christmas!' : xmasDays.textContent = daysLeft + ' day till Christmas!';
        }
    });
</script>

<Particles options='{particleOptions}'></Particles>

<div class='center'>
    <h1 id='xmas-days'>Days till christmas</h1>
</div>
