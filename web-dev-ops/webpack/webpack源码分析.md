# `webpack`源码分析

## 概述

- `webpack.js`
	- 初始化plugins
	- `WebpackOptionsApply.js`
- `Compiler.js`
- `Compilation.js`
- `NormalModule.js`

## `webpack.js`

### `WebpackOptionsApply.js`
```js
const AssetModulesPlugin = require("./asset/AssetModulesPlugin");
const JavascriptModulesPlugin = require("./javascript/JavascriptModulesPlugin");
const JsonModulesPlugin = require("./json/JsonModulesPlugin");
```
## `Compiler.js`

### hooks
```js
this.hooks = Object.freeze({
	/** @type {SyncHook<[]>} */
	initialize: new SyncHook([]),

	/** @type {SyncBailHook<[Compilation], boolean | undefined>} */
	shouldEmit: new SyncBailHook(["compilation"]),
	/** @type {AsyncSeriesHook<[Stats]>} */
	done: new AsyncSeriesHook(["stats"]),
	/** @type {SyncHook<[Stats]>} */
	afterDone: new SyncHook(["stats"]),
	/** @type {AsyncSeriesHook<[]>} */
	additionalPass: new AsyncSeriesHook([]),
	/** @type {AsyncSeriesHook<[Compiler]>} */
	beforeRun: new AsyncSeriesHook(["compiler"]),
	/** @type {AsyncSeriesHook<[Compiler]>} */
	run: new AsyncSeriesHook(["compiler"]),
	/** @type {AsyncSeriesHook<[Compilation]>} */
	emit: new AsyncSeriesHook(["compilation"]),
	/** @type {AsyncSeriesHook<[string, AssetEmittedInfo]>} */
	assetEmitted: new AsyncSeriesHook(["file", "info"]),
	/** @type {AsyncSeriesHook<[Compilation]>} */
	afterEmit: new AsyncSeriesHook(["compilation"]),

	/** @type {SyncHook<[Compilation, CompilationParams]>} */
	thisCompilation: new SyncHook(["compilation", "params"]),
	/** @type {SyncHook<[Compilation, CompilationParams]>} */
	compilation: new SyncHook(["compilation", "params"]),
	/** @type {SyncHook<[NormalModuleFactory]>} */
	normalModuleFactory: new SyncHook(["normalModuleFactory"]),
	/** @type {SyncHook<[ContextModuleFactory]>}  */
	contextModuleFactory: new SyncHook(["contextModuleFactory"]),

	/** @type {AsyncSeriesHook<[CompilationParams]>} */
	beforeCompile: new AsyncSeriesHook(["params"]),
	/** @type {SyncHook<[CompilationParams]>} */
	compile: new SyncHook(["params"]),
	/** @type {AsyncParallelHook<[Compilation]>} */
	make: new AsyncParallelHook(["compilation"]),
	/** @type {AsyncParallelHook<[Compilation]>} */
	finishMake: new AsyncSeriesHook(["compilation"]),
	/** @type {AsyncSeriesHook<[Compilation]>} */
	afterCompile: new AsyncSeriesHook(["compilation"]),

	/** @type {AsyncSeriesHook<[]>} */
	readRecords: new AsyncSeriesHook([]),
	/** @type {AsyncSeriesHook<[]>} */
	emitRecords: new AsyncSeriesHook([]),

	/** @type {AsyncSeriesHook<[Compiler]>} */
	watchRun: new AsyncSeriesHook(["compiler"]),
	/** @type {SyncHook<[Error]>} */
	failed: new SyncHook(["error"]),
	/** @type {SyncHook<[string | null, number]>} */
	invalid: new SyncHook(["filename", "changeTime"]),
	/** @type {SyncHook<[]>} */
	watchClose: new SyncHook([]),
	/** @type {AsyncSeriesHook<[]>} */
	shutdown: new AsyncSeriesHook([]),

	/** @type {SyncBailHook<[string, string, any[]], true>} */
	infrastructureLog: new SyncBailHook(["origin", "type", "args"]),

	// TODO the following hooks are weirdly located here
	// TODO move them for webpack 5
	/** @type {SyncHook<[]>} */
	environment: new SyncHook([]),
	/** @type {SyncHook<[]>} */
	afterEnvironment: new SyncHook([]),
	/** @type {SyncHook<[Compiler]>} */
	afterPlugins: new SyncHook(["compiler"]),
	/** @type {SyncHook<[Compiler]>} */
	afterResolvers: new SyncHook(["compiler"]),
	/** @type {SyncBailHook<[string, Entry], boolean>} */
	entryOption: new SyncBailHook(["context", "entry"])
});
```
## `Compilation.js`
### hooks
```js
this.hooks = Object.freeze({
	/** @type {SyncHook<[Module]>} */
	buildModule: new SyncHook(["module"]),
	/** @type {SyncHook<[Module]>} */
	rebuildModule: new SyncHook(["module"]),
	/** @type {SyncHook<[Module, WebpackError]>} */
	failedModule: new SyncHook(["module", "error"]),
	/** @type {SyncHook<[Module]>} */
	succeedModule: new SyncHook(["module"]),
	/** @type {SyncHook<[Module]>} */
	stillValidModule: new SyncHook(["module"]),

	/** @type {SyncHook<[Dependency, EntryOptions]>} */
	addEntry: new SyncHook(["entry", "options"]),
	/** @type {SyncHook<[Dependency, EntryOptions, Error]>} */
	failedEntry: new SyncHook(["entry", "options", "error"]),
	/** @type {SyncHook<[Dependency, EntryOptions, Module]>} */
	succeedEntry: new SyncHook(["entry", "options", "module"]),

	/** @type {SyncWaterfallHook<[(string[] | ReferencedExport)[], Dependency, RuntimeSpec]>} */
	dependencyReferencedExports: new SyncWaterfallHook([
		"referencedExports",
		"dependency",
		"runtime"
	]),

	/** @type {SyncHook<[ExecuteModuleArgument, ExecuteModuleContext]>} */
	executeModule: new SyncHook(["options", "context"]),
	/** @type {AsyncParallelHook<[ExecuteModuleArgument, ExecuteModuleContext]>} */
	prepareModuleExecution: new AsyncParallelHook(["options", "context"]),

	/** @type {AsyncSeriesHook<[Iterable<Module>]>} */
	finishModules: new AsyncSeriesHook(["modules"]),
	/** @type {AsyncSeriesHook<[Module]>} */
	finishRebuildingModule: new AsyncSeriesHook(["module"]),
	/** @type {SyncHook<[]>} */
	unseal: new SyncHook([]),
	/** @type {SyncHook<[]>} */
	seal: new SyncHook([]),

	/** @type {SyncHook<[]>} */
	beforeChunks: new SyncHook([]),
	/**
	 * The `afterChunks` hook is called directly after the chunks and module graph have
	 * been created and before the chunks and modules have been optimized. This hook is useful to
	 * inspect, analyze, and/or modify the chunk graph.
	 * @type {SyncHook<[Iterable<Chunk>]>}
	 */
	afterChunks: new SyncHook(["chunks"]),

	/** @type {SyncBailHook<[Iterable<Module>]>} */
	optimizeDependencies: new SyncBailHook(["modules"]),
	/** @type {SyncHook<[Iterable<Module>]>} */
	afterOptimizeDependencies: new SyncHook(["modules"]),

	/** @type {SyncHook<[]>} */
	optimize: new SyncHook([]),
	/** @type {SyncBailHook<[Iterable<Module>]>} */
	optimizeModules: new SyncBailHook(["modules"]),
	/** @type {SyncHook<[Iterable<Module>]>} */
	afterOptimizeModules: new SyncHook(["modules"]),

	/** @type {SyncBailHook<[Iterable<Chunk>, ChunkGroup[]]>} */
	optimizeChunks: new SyncBailHook(["chunks", "chunkGroups"]),
	/** @type {SyncHook<[Iterable<Chunk>, ChunkGroup[]]>} */
	afterOptimizeChunks: new SyncHook(["chunks", "chunkGroups"]),

	/** @type {AsyncSeriesHook<[Iterable<Chunk>, Iterable<Module>]>} */
	optimizeTree: new AsyncSeriesHook(["chunks", "modules"]),
	/** @type {SyncHook<[Iterable<Chunk>, Iterable<Module>]>} */
	afterOptimizeTree: new SyncHook(["chunks", "modules"]),

	/** @type {AsyncSeriesBailHook<[Iterable<Chunk>, Iterable<Module>]>} */
	optimizeChunkModules: new AsyncSeriesBailHook(["chunks", "modules"]),
	/** @type {SyncHook<[Iterable<Chunk>, Iterable<Module>]>} */
	afterOptimizeChunkModules: new SyncHook(["chunks", "modules"]),
	/** @type {SyncBailHook<[], boolean | undefined>} */
	shouldRecord: new SyncBailHook([]),

	/** @type {SyncHook<[Chunk, Set<string>, RuntimeRequirementsContext]>} */
	additionalChunkRuntimeRequirements: new SyncHook([
		"chunk",
		"runtimeRequirements",
		"context"
	]),
	/** @type {HookMap<SyncBailHook<[Chunk, Set<string>, RuntimeRequirementsContext]>>} */
	runtimeRequirementInChunk: new HookMap(
		() => new SyncBailHook(["chunk", "runtimeRequirements", "context"])
	),
	/** @type {SyncHook<[Module, Set<string>, RuntimeRequirementsContext]>} */
	additionalModuleRuntimeRequirements: new SyncHook([
		"module",
		"runtimeRequirements",
		"context"
	]),
	/** @type {HookMap<SyncBailHook<[Module, Set<string>, RuntimeRequirementsContext]>>} */
	runtimeRequirementInModule: new HookMap(
		() => new SyncBailHook(["module", "runtimeRequirements", "context"])
	),
	/** @type {SyncHook<[Chunk, Set<string>, RuntimeRequirementsContext]>} */
	additionalTreeRuntimeRequirements: new SyncHook([
		"chunk",
		"runtimeRequirements",
		"context"
	]),
	/** @type {HookMap<SyncBailHook<[Chunk, Set<string>, RuntimeRequirementsContext]>>} */
	runtimeRequirementInTree: new HookMap(
		() => new SyncBailHook(["chunk", "runtimeRequirements", "context"])
	),

	/** @type {SyncHook<[RuntimeModule, Chunk]>} */
	runtimeModule: new SyncHook(["module", "chunk"]),

	/** @type {SyncHook<[Iterable<Module>, any]>} */
	reviveModules: new SyncHook(["modules", "records"]),
	/** @type {SyncHook<[Iterable<Module>]>} */
	beforeModuleIds: new SyncHook(["modules"]),
	/** @type {SyncHook<[Iterable<Module>]>} */
	moduleIds: new SyncHook(["modules"]),
	/** @type {SyncHook<[Iterable<Module>]>} */
	optimizeModuleIds: new SyncHook(["modules"]),
	/** @type {SyncHook<[Iterable<Module>]>} */
	afterOptimizeModuleIds: new SyncHook(["modules"]),

	/** @type {SyncHook<[Iterable<Chunk>, any]>} */
	reviveChunks: new SyncHook(["chunks", "records"]),
	/** @type {SyncHook<[Iterable<Chunk>]>} */
	beforeChunkIds: new SyncHook(["chunks"]),
	/** @type {SyncHook<[Iterable<Chunk>]>} */
	chunkIds: new SyncHook(["chunks"]),
	/** @type {SyncHook<[Iterable<Chunk>]>} */
	optimizeChunkIds: new SyncHook(["chunks"]),
	/** @type {SyncHook<[Iterable<Chunk>]>} */
	afterOptimizeChunkIds: new SyncHook(["chunks"]),

	/** @type {SyncHook<[Iterable<Module>, any]>} */
	recordModules: new SyncHook(["modules", "records"]),
	/** @type {SyncHook<[Iterable<Chunk>, any]>} */
	recordChunks: new SyncHook(["chunks", "records"]),

	/** @type {SyncHook<[Iterable<Module>]>} */
	optimizeCodeGeneration: new SyncHook(["modules"]),

	/** @type {SyncHook<[]>} */
	beforeModuleHash: new SyncHook([]),
	/** @type {SyncHook<[]>} */
	afterModuleHash: new SyncHook([]),

	/** @type {SyncHook<[]>} */
	beforeCodeGeneration: new SyncHook([]),
	/** @type {SyncHook<[]>} */
	afterCodeGeneration: new SyncHook([]),

	/** @type {SyncHook<[]>} */
	beforeRuntimeRequirements: new SyncHook([]),
	/** @type {SyncHook<[]>} */
	afterRuntimeRequirements: new SyncHook([]),

	/** @type {SyncHook<[]>} */
	beforeHash: new SyncHook([]),
	/** @type {SyncHook<[Chunk]>} */
	contentHash: new SyncHook(["chunk"]),
	/** @type {SyncHook<[]>} */
	afterHash: new SyncHook([]),
	/** @type {SyncHook<[any]>} */
	recordHash: new SyncHook(["records"]),
	/** @type {SyncHook<[Compilation, any]>} */
	record: new SyncHook(["compilation", "records"]),

	/** @type {SyncHook<[]>} */
	beforeModuleAssets: new SyncHook([]),
	/** @type {SyncBailHook<[], boolean>} */
	shouldGenerateChunkAssets: new SyncBailHook([]),
	/** @type {SyncHook<[]>} */
	beforeChunkAssets: new SyncHook([]),
	// TODO webpack 6 remove
	/** @deprecated */
	additionalChunkAssets: createProcessAssetsHook(
		"additionalChunkAssets",
		Compilation.PROCESS_ASSETS_STAGE_ADDITIONAL,
		() => [this.chunks],
		"DEP_WEBPACK_COMPILATION_ADDITIONAL_CHUNK_ASSETS"
	),

	// TODO webpack 6 deprecate
	/** @deprecated */
	additionalAssets: createProcessAssetsHook(
		"additionalAssets",
		Compilation.PROCESS_ASSETS_STAGE_ADDITIONAL,
		() => []
	),
	// TODO webpack 6 remove
	/** @deprecated */
	optimizeChunkAssets: createProcessAssetsHook(
		"optimizeChunkAssets",
		Compilation.PROCESS_ASSETS_STAGE_OPTIMIZE,
		() => [this.chunks],
		"DEP_WEBPACK_COMPILATION_OPTIMIZE_CHUNK_ASSETS"
	),
	// TODO webpack 6 remove
	/** @deprecated */
	afterOptimizeChunkAssets: createProcessAssetsHook(
		"afterOptimizeChunkAssets",
		Compilation.PROCESS_ASSETS_STAGE_OPTIMIZE + 1,
		() => [this.chunks],
		"DEP_WEBPACK_COMPILATION_AFTER_OPTIMIZE_CHUNK_ASSETS"
	),
	// TODO webpack 6 deprecate
	/** @deprecated */
	optimizeAssets: processAssetsHook,
	// TODO webpack 6 deprecate
	/** @deprecated */
	afterOptimizeAssets: afterProcessAssetsHook,

	processAssets: processAssetsHook,
	afterProcessAssets: afterProcessAssetsHook,
	/** @type {AsyncSeriesHook<[CompilationAssets]>} */
	processAdditionalAssets: new AsyncSeriesHook(["assets"]),

	/** @type {SyncBailHook<[], boolean>} */
	needAdditionalSeal: new SyncBailHook([]),
	/** @type {AsyncSeriesHook<[]>} */
	afterSeal: new AsyncSeriesHook([]),

	/** @type {SyncWaterfallHook<[RenderManifestEntry[], RenderManifestOptions]>} */
	renderManifest: new SyncWaterfallHook(["result", "options"]),

	/** @type {SyncHook<[Hash]>} */
	fullHash: new SyncHook(["hash"]),
	/** @type {SyncHook<[Chunk, Hash, ChunkHashContext]>} */
	chunkHash: new SyncHook(["chunk", "chunkHash", "ChunkHashContext"]),

	/** @type {SyncHook<[Module, string]>} */
	moduleAsset: new SyncHook(["module", "filename"]),
	/** @type {SyncHook<[Chunk, string]>} */
	chunkAsset: new SyncHook(["chunk", "filename"]),

	/** @type {SyncWaterfallHook<[string, object, AssetInfo]>} */
	assetPath: new SyncWaterfallHook(["path", "options", "assetInfo"]),

	/** @type {SyncBailHook<[], boolean>} */
	needAdditionalPass: new SyncBailHook([]),

	/** @type {SyncHook<[Compiler, string, number]>} */
	childCompiler: new SyncHook([
		"childCompiler",
		"compilerName",
		"compilerIndex"
	]),

	/** @type {SyncBailHook<[string, LogEntry], true>} */
	log: new SyncBailHook(["origin", "logEntry"]),

	/** @type {SyncWaterfallHook<[WebpackError[]]>} */
	processWarnings: new SyncWaterfallHook(["warnings"]),
	/** @type {SyncWaterfallHook<[WebpackError[]]>} */
	processErrors: new SyncWaterfallHook(["errors"]),

	/** @type {HookMap<SyncHook<[Partial<NormalizedStatsOptions>, CreateStatsOptionsContext]>>} */
	statsPreset: new HookMap(() => new SyncHook(["options", "context"])),
	/** @type {SyncHook<[Partial<NormalizedStatsOptions>, CreateStatsOptionsContext]>} */
	statsNormalize: new SyncHook(["options", "context"]),
	/** @type {SyncHook<[StatsFactory, NormalizedStatsOptions]>} */
	statsFactory: new SyncHook(["statsFactory", "options"]),
	/** @type {SyncHook<[StatsPrinter, NormalizedStatsOptions]>} */
	statsPrinter: new SyncHook(["statsPrinter", "options"]),

	get normalModuleLoader() {
		return getNormalModuleLoader();
	}
});
```
## `NormalModule.js`

```js

/** @type {WeakMap<Compilation, NormalModuleCompilationHooks>} */
const compilationHooksMap = new WeakMap();

class NormalModule extends Module {
    static getCompilationHooks(compilation) {
    }
    
    /**
     * 模块加载
     */
    _doBuild(options, compilation, resolver, fs, hooks, callback) {
        runLoaders();
    }
    
    /**
     * 入口
     */
    build(options, compilation, resolver, fs, callback) {
        // 流程触发事件列表
        const hooks = NormalModule.getCompilationHooks(compilation);
        
        return this._doBuild(options, compilation, resolver, fs, hooks, err => {
    	    // 内容解析    
        }
    }
}
```

### hooks
```ts
type NormalModuleCompilationHooks = {
    loader: SyncHook<[object, NormalModule]>;
    beforeLoaders: SyncHook<[LoaderItem[], NormalModule, object]>;
    beforeParse: SyncHook<[NormalModule]>;
    beforeSnapshot: SyncHook<[NormalModule]>;
    readResourceForScheme: HookMap<AsyncSeriesBailHook<[string, NormalModule], string | Buffer>>;
    readResource: HookMap<AsyncSeriesBailHook<[object], string | Buffer>>;
    needBuild: AsyncSeriesBailHook<[NormalModule, NeedBuildContext], boolean>;
}
```

## `ContextModuleFactory.js`

```js
class ContextModuleFactory extends ModuleFactory {
	/**
	 * @param {ResolverFactory} resolverFactory resolverFactory
	 */
	constructor(resolverFactory) {
		super();
		/** @type {AsyncSeriesWaterfallHook<[TODO[], ContextModuleOptions]>} */
		const alternativeRequests = new AsyncSeriesWaterfallHook([
			"modules",
			"options"
		]);
		this.hooks = Object.freeze({
			/** @type {AsyncSeriesWaterfallHook<[TODO]>} */
			beforeResolve: new AsyncSeriesWaterfallHook(["data"]),
			/** @type {AsyncSeriesWaterfallHook<[TODO]>} */
			afterResolve: new AsyncSeriesWaterfallHook(["data"]),
			/** @type {SyncWaterfallHook<[string[]]>} */
			contextModuleFiles: new SyncWaterfallHook(["files"]),
			/** @type {FakeHook<Pick<AsyncSeriesWaterfallHook<[TODO[]]>, "tap" | "tapAsync" | "tapPromise" | "name">>} */
			alternatives: createFakeHook(
				{
					name: "alternatives",
					/** @type {AsyncSeriesWaterfallHook<[TODO[]]>["intercept"]} */
					intercept: interceptor => {
						throw new Error(
							"Intercepting fake hook ContextModuleFactory.hooks.alternatives is not possible, use ContextModuleFactory.hooks.alternativeRequests instead"
						);
					},
					/** @type {AsyncSeriesWaterfallHook<[TODO[]]>["tap"]} */
					tap: (options, fn) => {
						alternativeRequests.tap(options, fn);
					},
					/** @type {AsyncSeriesWaterfallHook<[TODO[]]>["tapAsync"]} */
					tapAsync: (options, fn) => {
						alternativeRequests.tapAsync(options, (items, _options, callback) =>
							fn(items, callback)
						);
					},
					/** @type {AsyncSeriesWaterfallHook<[TODO[]]>["tapPromise"]} */
					tapPromise: (options, fn) => {
						alternativeRequests.tapPromise(options, fn);
					}
				},
				"ContextModuleFactory.hooks.alternatives has deprecated in favor of ContextModuleFactory.hooks.alternativeRequests with an additional options argument.",
				"DEP_WEBPACK_CONTEXT_MODULE_FACTORY_ALTERNATIVES"
			),
			alternativeRequests
		});
		this.resolverFactory = resolverFactory;
	}
	
	/**
	 * @param {ModuleFactoryCreateData} data data object
	 * @param {function((Error | null)=, ModuleFactoryResult=): void} callback callback
	 * @returns {void}
	 */
	create(data, callback) {
	
   } 
}

```

### hooks

```js
this.hooks = Object.freeze({
	/** @type {AsyncSeriesWaterfallHook<[TODO]>} */
	beforeResolve: new AsyncSeriesWaterfallHook(["data"]),
	/** @type {AsyncSeriesWaterfallHook<[TODO]>} */
	afterResolve: new AsyncSeriesWaterfallHook(["data"]),
	/** @type {SyncWaterfallHook<[string[]]>} */
	contextModuleFiles: new SyncWaterfallHook(["files"]),
	/** @type {FakeHook<Pick<AsyncSeriesWaterfallHook<[TODO[]]>, "tap" | "tapAsync" | "tapPromise" | "name">>} */
	alternatives: createFakeHook(
		{
			name: "alternatives",
			/** @type {AsyncSeriesWaterfallHook<[TODO[]]>["intercept"]} */
			intercept: interceptor => {
				throw new Error(
					"Intercepting fake hook ContextModuleFactory.hooks.alternatives is not possible, use ContextModuleFactory.hooks.alternativeRequests instead"
				);
			},
			/** @type {AsyncSeriesWaterfallHook<[TODO[]]>["tap"]} */
			tap: (options, fn) => {
				alternativeRequests.tap(options, fn);
			},
			/** @type {AsyncSeriesWaterfallHook<[TODO[]]>["tapAsync"]} */
			tapAsync: (options, fn) => {
				alternativeRequests.tapAsync(options, (items, _options, callback) =>
					fn(items, callback)
				);
			},
			/** @type {AsyncSeriesWaterfallHook<[TODO[]]>["tapPromise"]} */
			tapPromise: (options, fn) => {
				alternativeRequests.tapPromise(options, fn);
			}
		},
		"ContextModuleFactory.hooks.alternatives has deprecated in favor of ContextModuleFactory.hooks.alternativeRequests with an additional options argument.",
		"DEP_WEBPACK_CONTEXT_MODULE_FACTORY_ALTERNATIVES"
	),
	alternativeRequests
});
```

## `NormalModuleFactory.js`
创建并配置Module

### hooks

```js
this.hooks = Object.freeze({
	/** @type {AsyncSeriesBailHook<[ResolveData], Module | false | void>} */
	resolve: new AsyncSeriesBailHook(["resolveData"]),
	/** @type {HookMap<AsyncSeriesBailHook<[ResourceDataWithData, ResolveData], true | void>>} */
	resolveForScheme: new HookMap(
		() => new AsyncSeriesBailHook(["resourceData", "resolveData"])
	),
	/** @type {HookMap<AsyncSeriesBailHook<[ResourceDataWithData, ResolveData], true | void>>} */
	resolveInScheme: new HookMap(
		() => new AsyncSeriesBailHook(["resourceData", "resolveData"])
	),
	/** @type {AsyncSeriesBailHook<[ResolveData], Module>} */
	factorize: new AsyncSeriesBailHook(["resolveData"]),
	/** @type {AsyncSeriesBailHook<[ResolveData], false | void>} */
	beforeResolve: new AsyncSeriesBailHook(["resolveData"]),
	/** @type {AsyncSeriesBailHook<[ResolveData], false | void>} */
	afterResolve: new AsyncSeriesBailHook(["resolveData"]),
	/** @type {AsyncSeriesBailHook<[ResolveData["createData"], ResolveData], Module | void>} */
	createModule: new AsyncSeriesBailHook(["createData", "resolveData"]),
	/** @type {SyncWaterfallHook<[Module, ResolveData["createData"], ResolveData], Module>} */
	module: new SyncWaterfallHook(["module", "createData", "resolveData"]),
	createParser: new HookMap(() => new SyncBailHook(["parserOptions"])),
	parser: new HookMap(() => new SyncHook(["parser", "parserOptions"])),
	createGenerator: new HookMap(
		() => new SyncBailHook(["generatorOptions"])
	),
	generator: new HookMap(
		() => new SyncHook(["generator", "generatorOptions"])
	),
	createModuleClass: new HookMap(
		() => new SyncBailHook(["createData", "resolveData"])
	)
});
```

### 订阅者

#### `javascript/JavascriptModulesPlugin.js`
提供不同type的Parser

#### `JavascriptGenerator.js`


## `JavascriptParser.js`

### hooks
```js
this.hooks = Object.freeze({
	/** @type {HookMap<SyncBailHook<[UnaryExpression], BasicEvaluatedExpression | undefined | null>>} */
	evaluateTypeof: new HookMap(() => new SyncBailHook(["expression"])),
	/** @type {HookMap<SyncBailHook<[Expression], BasicEvaluatedExpression | undefined | null>>} */
	evaluate: new HookMap(() => new SyncBailHook(["expression"])),
	/** @type {HookMap<SyncBailHook<[Identifier | ThisExpression | MemberExpression | MetaProperty], BasicEvaluatedExpression | undefined | null>>} */
	evaluateIdentifier: new HookMap(() => new SyncBailHook(["expression"])),
	/** @type {HookMap<SyncBailHook<[Identifier | ThisExpression | MemberExpression], BasicEvaluatedExpression | undefined | null>>} */
	evaluateDefinedIdentifier: new HookMap(
		() => new SyncBailHook(["expression"])
	),
	/** @type {HookMap<SyncBailHook<[NewExpression], BasicEvaluatedExpression | undefined | null>>} */
	evaluateNewExpression: new HookMap(
		() => new SyncBailHook(["expression"])
	),
	/** @type {HookMap<SyncBailHook<[CallExpression], BasicEvaluatedExpression | undefined | null>>} */
	evaluateCallExpression: new HookMap(
		() => new SyncBailHook(["expression"])
	),
	/** @type {HookMap<SyncBailHook<[CallExpression, BasicEvaluatedExpression | undefined], BasicEvaluatedExpression | undefined | null>>} */
	evaluateCallExpressionMember: new HookMap(
		() => new SyncBailHook(["expression", "param"])
	),
	/** @type {HookMap<SyncBailHook<[Expression | Declaration | PrivateIdentifier, number], boolean | void>>} */
	isPure: new HookMap(
		() => new SyncBailHook(["expression", "commentsStartPosition"])
	),
	/** @type {SyncBailHook<[Statement | ModuleDeclaration], boolean | void>} */
	preStatement: new SyncBailHook(["statement"]),

	/** @type {SyncBailHook<[Statement | ModuleDeclaration], boolean | void>} */
	blockPreStatement: new SyncBailHook(["declaration"]),
	/** @type {SyncBailHook<[Statement | ModuleDeclaration], boolean | void>} */
	statement: new SyncBailHook(["statement"]),
	/** @type {SyncBailHook<[IfStatement], boolean | void>} */
	statementIf: new SyncBailHook(["statement"]),
	/** @type {SyncBailHook<[Expression, ClassExpression | ClassDeclaration], boolean | void>} */
	classExtendsExpression: new SyncBailHook([
		"expression",
		"classDefinition"
	]),
	/** @type {SyncBailHook<[MethodDefinition | PropertyDefinition | StaticBlock, ClassExpression | ClassDeclaration], boolean | void>} */
	classBodyElement: new SyncBailHook(["element", "classDefinition"]),
	/** @type {SyncBailHook<[Expression, MethodDefinition | PropertyDefinition, ClassExpression | ClassDeclaration], boolean | void>} */
	classBodyValue: new SyncBailHook([
		"expression",
		"element",
		"classDefinition"
	]),
	/** @type {HookMap<SyncBailHook<[LabeledStatement], boolean | void>>} */
	label: new HookMap(() => new SyncBailHook(["statement"])),
	/** @type {SyncBailHook<[ImportDeclaration, ImportSource], boolean | void>} */
	import: new SyncBailHook(["statement", "source"]),
	/** @type {SyncBailHook<[ImportDeclaration, ImportSource, string, string], boolean | void>} */
	importSpecifier: new SyncBailHook([
		"statement",
		"source",
		"exportName",
		"identifierName"
	]),
	/** @type {SyncBailHook<[ExportNamedDeclaration | ExportAllDeclaration], boolean | void>} */
	export: new SyncBailHook(["statement"]),
	/** @type {SyncBailHook<[ExportNamedDeclaration | ExportAllDeclaration, ImportSource], boolean | void>} */
	exportImport: new SyncBailHook(["statement", "source"]),
	/** @type {SyncBailHook<[ExportNamedDeclaration | ExportAllDeclaration, Declaration], boolean | void>} */
	exportDeclaration: new SyncBailHook(["statement", "declaration"]),
	/** @type {SyncBailHook<[ExportDefaultDeclaration, Declaration], boolean | void>} */
	exportExpression: new SyncBailHook(["statement", "declaration"]),
	/** @type {SyncBailHook<[ExportNamedDeclaration | ExportAllDeclaration, string, string, number | undefined], boolean | void>} */
	exportSpecifier: new SyncBailHook([
		"statement",
		"identifierName",
		"exportName",
		"index"
	]),
	/** @type {SyncBailHook<[ExportNamedDeclaration | ExportAllDeclaration, ImportSource, string, string, number | undefined], boolean | void>} */
	exportImportSpecifier: new SyncBailHook([
		"statement",
		"source",
		"identifierName",
		"exportName",
		"index"
	]),
	/** @type {SyncBailHook<[VariableDeclarator, Statement], boolean | void>} */
	preDeclarator: new SyncBailHook(["declarator", "statement"]),
	/** @type {SyncBailHook<[VariableDeclarator, Statement], boolean | void>} */
	declarator: new SyncBailHook(["declarator", "statement"]),
	/** @type {HookMap<SyncBailHook<[Declaration], boolean | void>>} */
	varDeclaration: new HookMap(() => new SyncBailHook(["declaration"])),
	/** @type {HookMap<SyncBailHook<[Declaration], boolean | void>>} */
	varDeclarationLet: new HookMap(() => new SyncBailHook(["declaration"])),
	/** @type {HookMap<SyncBailHook<[Declaration], boolean | void>>} */
	varDeclarationConst: new HookMap(() => new SyncBailHook(["declaration"])),
	/** @type {HookMap<SyncBailHook<[Declaration], boolean | void>>} */
	varDeclarationVar: new HookMap(() => new SyncBailHook(["declaration"])),
	/** @type {HookMap<SyncBailHook<[Identifier], boolean | void>>} */
	pattern: new HookMap(() => new SyncBailHook(["pattern"])),
	/** @type {HookMap<SyncBailHook<[Expression], boolean | void>>} */
	canRename: new HookMap(() => new SyncBailHook(["initExpression"])),
	/** @type {HookMap<SyncBailHook<[Expression], boolean | void>>} */
	rename: new HookMap(() => new SyncBailHook(["initExpression"])),
	/** @type {HookMap<SyncBailHook<[AssignmentExpression], boolean | void>>} */
	assign: new HookMap(() => new SyncBailHook(["expression"])),
	/** @type {HookMap<SyncBailHook<[AssignmentExpression, string[]], boolean | void>>} */
	assignMemberChain: new HookMap(
		() => new SyncBailHook(["expression", "members"])
	),
	/** @type {HookMap<SyncBailHook<[Expression], boolean | void>>} */
	typeof: new HookMap(() => new SyncBailHook(["expression"])),
	/** @type {SyncBailHook<[ImportExpression], boolean | void>} */
	importCall: new SyncBailHook(["expression"]),
	/** @type {SyncBailHook<[Expression], boolean | void>} */
	topLevelAwait: new SyncBailHook(["expression"]),
	/** @type {HookMap<SyncBailHook<[CallExpression], boolean | void>>} */
	call: new HookMap(() => new SyncBailHook(["expression"])),
	/** Something like "a.b()" */
	/** @type {HookMap<SyncBailHook<[CallExpression, string[], boolean[], Range[]], boolean | void>>} */
	callMemberChain: new HookMap(
		() =>
			new SyncBailHook([
				"expression",
				"members",
				"membersOptionals",
				"memberRanges"
			])
	),
	/** Something like "a.b().c.d" */
	/** @type {HookMap<SyncBailHook<[Expression, string[], CallExpression, string[]], boolean | void>>} */
	memberChainOfCallMemberChain: new HookMap(
		() =>
			new SyncBailHook([
				"expression",
				"calleeMembers",
				"callExpression",
				"members"
			])
	),
	/** Something like "a.b().c.d()"" */
	/** @type {HookMap<SyncBailHook<[CallExpression, string[], CallExpression, string[]], boolean | void>>} */
	callMemberChainOfCallMemberChain: new HookMap(
		() =>
			new SyncBailHook([
				"expression",
				"calleeMembers",
				"innerCallExpression",
				"members"
			])
	),
	/** @type {SyncBailHook<[ChainExpression], boolean | void>} */
	optionalChaining: new SyncBailHook(["optionalChaining"]),
	/** @type {HookMap<SyncBailHook<[NewExpression], boolean | void>>} */
	new: new HookMap(() => new SyncBailHook(["expression"])),
	/** @type {SyncBailHook<[BinaryExpression], boolean | void>} */
	binaryExpression: new SyncBailHook(["binaryExpression"]),
	/** @type {HookMap<SyncBailHook<[Expression], boolean | void>>} */
	expression: new HookMap(() => new SyncBailHook(["expression"])),
	/** @type {HookMap<SyncBailHook<[MemberExpression, string[], boolean[], Range[]], boolean | void>>} */
	expressionMemberChain: new HookMap(
		() =>
			new SyncBailHook([
				"expression",
				"members",
				"membersOptionals",
				"memberRanges"
			])
	),
	/** @type {HookMap<SyncBailHook<[MemberExpression, string[]], boolean | void>>} */
	unhandledExpressionMemberChain: new HookMap(
		() => new SyncBailHook(["expression", "members"])
	),
	/** @type {SyncBailHook<[ConditionalExpression], boolean | void>} */
	expressionConditionalOperator: new SyncBailHook(["expression"]),
	/** @type {SyncBailHook<[LogicalExpression], boolean | void>} */
	expressionLogicalOperator: new SyncBailHook(["expression"]),
	/** @type {SyncBailHook<[Program, Comment[]], boolean | void>} */
	program: new SyncBailHook(["ast", "comments"]),
	/** @type {SyncBailHook<[Program, Comment[]], boolean | void>} */
	finish: new SyncBailHook(["ast", "comments"])
});
```