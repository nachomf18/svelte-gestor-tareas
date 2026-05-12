<style>
    :global(body) {
        font-family: 'Segoe UI', sans-serif;
        padding: 30px;
        display: flex;
        flex-direction: column;
        align-items: center;
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        min-height: 100vh;
    }

    #nueva_tarea {
        margin-bottom: 30px;
        display: flex;
        gap: 10px;
        width: 100%;
        justify-content: center;
    }

    #nueva_tarea input {
        width: 400px;
        padding: 12px 15px;
        border: 2px solid #ccc;
        border-radius: 8px;
        font-size: 16px;
        transition: all 0.3s ease;
    }

    #nueva_tarea input:focus {
        outline: none;
        border-color: #667eea;
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.5);
    }

    #nueva_tarea button {
        padding: 12px 25px;
        background-color: #667eea;
        color: white;
        border: none;
        border-radius: 8px;
        font-size: 16px;
        cursor: pointer;
        transition: all 0.3s ease;
    }

    #nueva_tarea button:hover {
        background-color: #5568d3;
        transform: translateY(-2px);
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.5);
    }

    #wrapper {
        display: grid;
        grid-template-columns: repeat(2, 1fr);
        gap: 30px;
        width: 100%;
        max-width: 1600px;
    }

    .lista {
        background-color: white;
        padding: 25px;
        border-radius: 12px;
        box-shadow: 0 8px 24px rgba(0, 0, 0, 0.5);
    }

    .lista h2 {
        color: #333;
        text-align: center;
        margin-bottom: 20px;
        margin-top: 0;
    }

    hr {
        border: none;
        border-top: 2px solid #eee;
        width: 100%;
    }

    .tareaSinTerminar {
        background-color: #f8f9fa;
        padding: 50px 15px;
        margin: 15px 0;
        border-radius: 8px;
        display: flex;
        justify-content: space-between;
        align-items: center;
        gap: 50px;
        transition: all 0.3s ease;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
    }

    .tareaSinTerminar:hover {
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.4);
        background-color: #fff;
    }

    .tareaTerminada {
        background-color: #e9ecef;
        padding: 30px 15px;
        margin: 15px 0;
        border-radius: 8px;
        display: flex;
        justify-content: center;
        align-items: center;
        gap: 30px;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.3);
        text-decoration: line-through;
        color: #666;
    }

    .accionesPrioridad {
        display: flex;
        gap: 8px;
    }

    button {
        padding: 10px 15px;
        border: none;
        border-radius: 8px;
        cursor: pointer;
        font-size: 14px;
        transition: all 0.2s ease;
        background-color: #667eea;
        color: white;
        font-weight: bold;
    }

    .tareaSinTerminar button:hover {
        background-color: #5568d3;
        transform: scale(1.05);
    }

    .tareaTerminada button {
        background-color: #dc3545;
        color: white;
        padding: 10px 15px;
    }

    .tareaTerminada button:hover {
        background-color: #c82333;
        transform: scale(1.05);
    }
</style>

<script>
    import { onMount } from 'svelte';
    import { flip } from 'svelte/animate';

    const tareas = $state([]);

    function updateStorage() {
        localStorage.setItem("tareas", JSON.stringify(tareas));
    }

    onMount(() => {
        const tareasGuardadas = localStorage.getItem("tareas");

        if (tareasGuardadas) {
            tareas.push(...JSON.parse(tareasGuardadas));
        }
    });

    function addTarea() {
        let inputTarea = document.getElementById("tarea");

        if (inputTarea.value.trim() !== "") {
            tareas.push({
                titulo: inputTarea.value,
                completada: false,
            });

            updateStorage();
            inputTarea.value = "";
        }
    }

    function tareaCompletada(e, tarea) {
        e.stopPropagation();
        const index = tareas.indexOf(tarea);
        tareas[index].completada = true;
        updateStorage();
    }

    function eliminarTarea(e, tarea) {
        e.stopPropagation();
        const index = tareas.indexOf(tarea);
        tareas.splice(index, 1);
        updateStorage();
    }   

    function moverPrioridad(e, tarea, mover) {
        e.stopPropagation();
        const indexActual = tareas.indexOf(tarea);
        if (indexActual === -1) return;

        let indexNuevo = indexActual + mover;

        while (indexNuevo >= 0 && indexNuevo < tareas.length && tareas[indexNuevo].completada) {
            indexNuevo += mover;
        }

        if (indexNuevo < 0 || indexNuevo >= tareas.length) return;

        tareas[indexActual] = tareas[indexNuevo];
        tareas[indexNuevo] = tarea;
        updateStorage();
    }
</script>

<div id="nueva_tarea">
    <input type="text" id="tarea" placeholder="Escribe una tarea">
    <button onclick={addTarea}>Añadir tarea</button>
</div>

<div id="wrapper">
    <div id="tareas_sin_terminar" class="lista">
        <h2>Tareas pendientes</h2>
        <hr>
        {#each tareas.filter(t => !t.completada) as tarea, index (tarea)}
            <div class="tareaSinTerminar" animate:flip={{duration: 300}}>
                <div class="accionesPrioridad">
                {#if index > 0}
                    <button onclick={(e) => moverPrioridad(e, tarea, -1)}>⬆️</button>
                {/if}
                {#if index < tareas.filter(t => !t.completada).length - 1} 
                    <button onclick={(e) => moverPrioridad(e, tarea, 1)}>⬇️</button>
                {/if}
                </div>
                <div>
                    {tarea.titulo}
                </div>
                <div>
                    <button onclick={(e) => tareaCompletada(e, tarea)}>Terminar </button>
                </div>
            </div>
        {/each}
    </div>
    
    <div id="tareas_terminadas" class="lista">
        <h2>Tareas completadas</h2>
        <hr>
        {#each tareas.filter(t => t.completada) as tarea, index}
            <div class="tareaTerminada">
                <div>
                    {tarea.titulo}
                </div>
                <div>
                    <button onclick={(e) => eliminarTarea(e, tarea)}>Eliminar</button>
                </div>
            </div>
        {/each}
    </div>
</div>