<script>
import { onMount } from 'svelte';
import { updateCodeStore } from '../code-store.js';
import { graphs } from '../graphs.js';
import Editor from '../components/Editor.svelte';
import Config from '../components/Config.svelte';
import View from '../components/View.svelte';
import Card from '../components/Card.svelte';
import Tag from '../components/Tag.svelte';
import Links from '../components/Links.svelte';
import { fromUrl } from '../code-store.js';
// import pkg from '@mermaid-js/mermaid/package.json'
import pkg from '@mermaid/package.json'

export let mermaidVersion = pkg.version

function loadGraph(graphId) {
	loadSampleDiagram(graphId);
}

onMount(async () => {
	ga('send', 'pageview');
	ga('send', 'event', 'version', mermaidVersion, mermaidVersion);
	fromUrl(params.data);
});

	// export let code = '';
	// export let classes = '';

	// export let error = {};
	// export let token = '';
	// export let expected = '';
	export let params = {};

	function loadDependencyGraph(){
		loadSampleDiagram('DependencyGraph');
	}
	function loadSequenceDiagram(){
		loadSampleDiagram('SequenceDiagram');
	}
	function loadClassDiagram(){
		loadSampleDiagram('ClassDiagram');
	}
	function loadStateDiagram(){
		loadSampleDiagram('StateDiagram');
	}
	function loadGanttChart(){
		loadSampleDiagram('GanttChart');
	}
	function loadPieChart(){
		loadSampleDiagram('PieChart');
	}
	
    function loadSampleDiagram(graphId){
		let code = graphs.find(graph => graph.id == graphId).code;
		let newState = { code, mermaid: { theme: 'default' }, updateEditor:true };
		updateCodeStore(newState);
	}	

</script>

<style>
	#editor-root {
		display: flex;
		height: 100%;
	}

	#col1 {
		width: 35%;
	}
	#col2 {
		width: 65%;
		padding-left: 32px;
	}
	#app-title {
		border-bottom:  1px solid lightgrey;
		padding-bottom: 32px;
		margin-bottom: 32px;
		font-size: 28px;
		font-weight: 400;
		margin-top: 0;
	}

	#power {
		width: 100%;
    display: flex;
    justify-content: flex-end;
		align-items: center;
	}

	#sampleLoader {
		padding-bottom: 10px;
		padding-left:10px;
		
	}
	.botton-container{
		margin-top:5px;
	}

</style>
<div>
	<h1 id="app-title">Mermaid Live Editor</h1>
	<div id="editor-root">
		<div id="col1">
			<Card title="Code" noPadding="true">
			<div id="sampleLoader">Graphs :<br/>
				<div class="botton-container">
				{#each graphs as graph, i}
					<button on:click={() => loadGraph(graph.id)}>{graph.name}</button>
				{/each}
			</div>
			</div>
				<Editor data={params.data}/>
			</Card>
			<Card title="Mermaid Configuration" ><Config /></Card>
			<Card title='Links'>
				<Links />
				<ul className='marketing-links'>
					<li>
						<a href='https://mermaid-js.github.io/mermaid' target='_blank'>
							Mermaid Documentation
						</a>
					</li>
					<li>
						<a href='https://github.com/mermaid-js/mermaid' target='_blank'>
							Mermaid on GitHub
						</a>
					</li>
					<li>
						<a
							href='https://github.com/mermaid-js/mermaid-live-editor'
							target='_blank'
						>
							Live Editor on GitHub
						</a>
					</li>
					<li>
						<a
							href='https://github.com/mermaid-js/mermaid.cli'
							target='_blank'
						>
							Mermaid CLI
						</a>
					</li>
				</ul>
			</Card>
		</div>
		<div id="col2">
			<Card title="Preview"><View /></Card>
			<!-- <Card title="Actions"></Card>
			<div id="power">
				Powered by mermaid <Tag color='green'>{mermaidVersion}</Tag>
			</div> -->
		</div>
	</div>
</div>