# vscode构建问题日志

源码修改任务：将2019年删除的onDidExecuteCommand API加回

fork仓：https://github.com/Katock-Cricket/vscode-restoreAPIs

调试、编译、打包流程遵循：https://github.com/microsoft/vscode/wiki/How-to-Contribute

环境：Windows11(x64) MSVC

打包一次要60分钟，然后报错比较抽象很难解，js中间代码出现语法错误，可能是gulp等构建工具版本的问题导致模板填充出错，也有可能是其他环境问题，debug一次至少一个小时很费劲。

完整log：

```
(base) PS D:\LearningMaterials\智能软件工程组\华为@workspace@codechat\意图预测\iseg-ide-sub1\vscode-restoreAPIs> npm run gulp vscode-win32-x64-min                                                                           
> code-oss-dev@1.97.0 gulp
> node --max-old-space-size=8192 ./node_modules/gulp/bin/gulp.js vscode-win32-x64-min

[12:32:00] Using gulpfile D:\LearningMaterials\智能软件工程组\华为@workspace@codechat\意图预测\iseg-ide-sub1\vscode-restoreAPIs\gulpfile.js
[12:32:00] Starting 'vscode-win32-x64-min'...
[12:32:00] Starting clean-out-build ...
[12:32:04] Finished clean-out-build after 3718 ms
[12:32:04] Starting build-date-file ...
[12:32:04] Finished build-date-file after 3 ms
[12:32:04] Starting compile-api-proposal-names ...
[12:32:04] Starting compilation api-proposal-names...
[12:32:04] Finished compilation api-proposal-names with 0 errors after 244 ms
[12:32:04] Finished compile-api-proposal-names after 268 ms
[12:32:04] Starting compile-src ...
[12:32:22] [mangler] Done collecting. Classes: 8230. Exported symbols: 10005
[12:32:23] [mangler] Done creating class replacements
[12:32:23] [mangler] Starting prepare rename edits
[12:32:24] Starting compilation...
[13:22:00] [mangler] Done preparing edits: 4052 files
[13:22:07] [mangler] Done: 5087.001kb saved, memory-usage: {"total_heap_size":2878271488,"total_heap_size_executable":8368128,"total_physical_size":2878271488,"total_available_size":5814947112,"used_heap
_size":2779022648,"heap_size_limit":8640266240,"malloced_memory":1089584,"peak_malloced_memory":20449128,"does_zap_garbage":0,"number_of_native_contexts":3,"number_of_detached_contexts":0,"total_global_handles_size":4947968,"used_global_handles_size":1939584,"external_memory":115579691}
[13:26:55] Finished compilation with 0 errors after 3271506 ms
[13:26:56] Finished compile-src after 3291392 ms
[13:26:56] Starting compile-build ...
[13:26:56] Finished compile-build after 0 ms
[13:26:56] Starting clean-extensions-build ...
[13:26:56] Finished clean-extensions-build after 674 ms
[13:26:56] Starting bundle-marketplace-extensions-build ...
[13:26:56] [extensions] ms-vscode.js-debug-companion@1.1.3 up to date ✔︎
[13:26:56] [extensions] ms-vscode.js-debug@1.96.0 up to date ✔︎
[13:26:56] [extensions] ms-vscode.vscode-js-profile-table@1.0.10 up to date ✔︎
[13:26:57] Finished bundle-marketplace-extensions-build after 427 ms
[13:26:57] Starting bundle-non-native-extensions-build ...
[13:28:46] Bundled extension: simple-browser\extension.webpack.config.js...
[13:28:46] Bundled extension: search-result\extension.webpack.config.js...
[13:28:46] Bundled extension: terminal-suggest\extension.webpack.config.js...
[13:28:47] Bundled extension: debug-auto-launch\extension.webpack.config.js...
[13:28:47] Bundled extension: grunt\extension.webpack.config.js...
[13:28:48] Bundled extension: debug-server-ready\extension.webpack.config.js...
[13:28:48] Bundled extension: gulp\extension.webpack.config.js...
[13:28:49] Bundled extension: jake\extension.webpack.config.js...
[13:29:01] Bundled extension: markdown-math\extension.webpack.config.js...
[13:29:05] Bundled extension: tunnel-forwarding\extension.webpack.config.js...
[13:29:08] Bundled extension: references-view\extension.webpack.config.js...
[13:29:08] Bundled extension: git-base\extension.webpack.config.js...
[13:29:10] Bundled extension: media-preview\extension.webpack.config.js...
[13:29:18] Bundled extension: ipynb\extension.webpack.config.js...
[13:29:33] Bundled extension: php-language-features\extension.webpack.config.js...
[13:29:43] Bundled extension: extension-editing\extension.webpack.config.js...
[13:29:48] Bundled extension: emmet\extension.webpack.config.js...
[13:30:07] Bundled extension: npm\extension.webpack.config.js...
[13:30:07] Bundled extension: css-language-features\extension.webpack.config.js...
[13:30:10] Bundled extension: json-language-features\server\extension.webpack.config.js...
[13:30:11] Bundled extension: css-language-features\server\extension.webpack.config.js...
[13:30:12] Bundled extension: html-language-features\server\extension.webpack.config.js...
[13:30:12] Bundled extension: merge-conflict\extension.webpack.config.js...
[13:30:13] Bundled extension: git\extension.webpack.config.js...
[13:30:13] Bundled extension: github-authentication\extension.webpack.config.js...
[13:30:22] Bundled extension: configuration-editing\extension.webpack.config.js...
[13:30:25] Bundled extension: typescript-language-features\extension.webpack.config.js...
[13:30:26] Bundled extension: html-language-features\extension.webpack.config.js...
[13:30:27] Bundled extension: json-language-features\extension.webpack.config.js...
[13:30:29] Bundled extension: github\extension.webpack.config.js...
[13:30:38] Bundled extension: markdown-language-features\extension.webpack.config.js...
[13:30:38] Finished bundle-non-native-extensions-build after 221322 ms
[13:30:38] Starting compile-non-native-extensions-build ...
[13:30:38] Finished compile-non-native-extensions-build after 0 ms
[13:30:38] Starting compile-extension-media-build ...
[13:30:39] Finished esbuilding extension media D:\LearningMaterials\智能软件工程组\华为@workspace@codechat\意图预测\iseg-ide-sub1\vscode-restoreAPIs\extensions\ipynb\esbuild.js with 0 errors.
[13:30:39] Finished esbuilding extension media D:\LearningMaterials\智能软件工程组\华为@workspace@codechat\意图预测\iseg-ide-sub1\vscode-restoreAPIs\extensions\notebook-renderers\esbuild.js with 0 errors.
[13:30:39] Finished esbuilding extension media D:\LearningMaterials\智能软件工程组\华为@workspace@codechat\意图预测\iseg-ide-sub1\vscode-restoreAPIs\extensions\markdown-math\esbuild.js with 0 errors.    
[13:30:39] Finished esbuilding extension media D:\LearningMaterials\智能软件工程组\华为@workspace@codechat\意图预测\iseg-ide-sub1\vscode-restoreAPIs\extensions\markdown-language-features\esbuild-preview.js with 0 errors.
[13:30:39] Finished esbuilding extension media D:\LearningMaterials\智能软件工程组\华为@workspace@codechat\意图预测\iseg-ide-sub1\vscode-restoreAPIs\extensions\markdown-language-features\esbuild-notebook.js with 0 errors.
[13:30:39] Finished esbuilding extension media D:\LearningMaterials\智能软件工程组\华为@workspace@codechat\意图预测\iseg-ide-sub1\vscode-restoreAPIs\extensions\simple-browser\esbuild-preview.js with 0 errors.
[13:30:39] Finished compile-extension-media-build after 1211 ms
[13:30:39] Starting clean-out-vscode ...
[13:30:39] Finished clean-out-vscode after 1 ms
[13:30:39] Starting bundle-vscode ...
[13:30:39] Bundled entry point: vs/editor/common/services/editorSimpleWorkerMain...
[13:30:40] Bundled entry point: vs/workbench/api/worker/extensionHostWorkerMain...
[13:30:40] Bundled entry point: vs/workbench/contrib/notebook/common/services/notebookSimpleWorkerMain...
[13:30:40] Bundled entry point: vs/workbench/services/languageDetection/browser/languageDetectionSimpleWorkerMain...
[13:30:40] Bundled entry point: vs/workbench/services/search/worker/localFileSearchMain...
[13:30:40] Bundled entry point: vs/platform/profiling/electron-sandbox/profileAnalysisWorkerMain...
[13:30:40] Bundled entry point: vs/workbench/contrib/output/common/outputLinkComputerMain...
[13:30:40] Bundled entry point: vs/workbench/services/textMate/browser/backgroundTokenization/worker/textMateTokenizationWorker.workerMain...
[13:30:40] Bundled entry point: vs/workbench/contrib/debug/node/telemetryApp...
[13:30:40] Bundled entry point: vs/platform/files/node/watcher/watcherMain...
[13:30:40] Bundled entry point: vs/platform/terminal/node/ptyHostMain...
[13:30:40] Bundled entry point: vs/workbench/api/node/extensionHostProcess...
[13:30:40] Bundled entry point: vs/workbench/workbench.desktop.main...
[13:30:40] Bundled entry point: vs/code/node/cliProcessMain...
[13:30:40] Bundled entry point: vs/code/electron-utility/sharedProcess/sharedProcessMain...
[13:30:40] Bundled entry point: vs/code/electron-sandbox/processExplorer/processExplorerMain...
X [ERROR] Expected ")" but found ":"

    out-build/vs/workbench/api/common/extHostTelemetry.js:42:54:
      42 │         this.n = t.createLogger(this.m2763idnull, name: localize('extensionTelemetryLog', "Extension Telemetry{0}", this.j ? ' (Not Sent)' : ''), hidden: true });
         │                                                       ^
         ╵                                                       )

X [ERROR] Expected ")" but found ".2768"

    out-build/vs/workbench/api/common/extHostWorkspace.js:247:44:
      247 │                 this.q.$showMessage(Severity.2768(null"Extension '{0}' failed to update workspace folders: {1}", extName, error.toString()), options, []);
          │                                             ~~~~~
          ╵                                             )

[13:30:40] Bundled entry point: vs/code/electron-sandbox/workbench/workbench...
[13:30:40] Bundled entry point: vs/code/electron-sandbox/processExplorer/processExplorer...
[13:30:40] Bundled entry point: main...
X [ERROR] Syntax error "l"

    out-build/vs/code/node/cliProcessMain.js:113:75:
      113 │         const logger = this.B(loggerService.createLogger('cli', { name: 175localizenull'cli', "CLI") }));
          ╵                                                                            ^

X [ERROR] Expected ":" but found ".7424"

    out-build/vs/workbench/contrib/files/electron-sandbox/fileActions.contribution.js:24:35:
      24 │ const REVEAL_IN_OS_LABEL = $l ? nls.7424'revealInWindows', "Reveal in File Explorer") : $m 7425localize2('revealInMac', "Reveal in Finder") 7426('openContainer', "Open Containing Folder");    
         │                                    ~~~~~X X 
         ╵                                    [[:ERROR
                                                                                                                                                                                                           
ERROR]] X  Syntax error "u"[Syntax error "u"

    out-build/vs/workbench/services/configurationResolver/common/variableResolver.js:147:46:
ERROR]      147 │             throw new $oY(variableKind, 12631n

    out-build/vs/code/electron-utility/sharedProcess/sharedProcessMain.js:184:86:
       184 │         const logger = this.B(loggerService.createLogger('sharedprocess', { name: 174nExpected "}" but found "'join.textFiles'"ull"Variable {0} can not be resolved. Please open an editor.", match)));
          ╵                                               ull"Shared") }));
          ╵

    out-build/vs/workbench/services/textfile/electron-sandbox/nativeTextFileService.js:44:76:
^^      44 │         this.B(this.h.onWillShutdown(event => event.join(this.W(), { id13131



'join.textFiles'null'join.textFiles', "Saving text files") })));
         │                                                                             X ~~~~~~~~~~~~~~~~[
         ╵                                                                             ERROR}]

 Expected ";" but found "'unknownError'"

    out-build/vs/platform/files/common/files.js:186:26:
X       186 │         return $xm1910null['unknownError'ERROR, "Unknown Error"), FileSystemProviderErrorCode.Unknown); // https://github.com/microsoft/vscode/issues/72798
          │                           ]~~~~~~~~~~~~~~ 
          ╵                           Syntax error "l";

    out-build/vs/workbench/electron-sandbox/desktop.contribution.js:77:71:


      77 │             { handler: $muc, id: 'workbench.action.newWindowTab', 12347X localize2('newTab', 'New Window Tab') },
         ╵                                                                        [^ERROR

] X Syntax error "u"[
                                                                                                                                                                                                           
    out-build/vs/workbench/contrib/tasks/common/tasks.js:12:33:                                                                                                                                            
ERROR      12 │ export const $cQ = new $nn(10348n] ull"Whether a task is currently running."));
         ╵                                  Syntax error "n"^



    out-build/vs/workbench/services/extensions/electron-sandbox/nativeExtensionService.js:118:28:
      118 │                 this.M.12899null"The extension host terminated unexpectedly. Restarting..."), { hideAfter: 5000 });
          ╵                             ^

X [ERROR] Expected ";" but found "'remoteTunnel.category'"

    out-build/vs/workbench/contrib/remoteTunnel/electron-sandbox/remoteTunnel.contribution.js:43:21:
      43 │ export const $7vc9341'remoteTunnel.category', 'Remote Tunnels');
         │                      ~~~~~~~~~~~~~~~~~~~~~~~
         ╵                      ;

X [ERROR] Syntax error "u"

    out-build/vs/workbench/services/auxiliaryWindow/electron-sandbox/auxiliaryWindowService.js:69:33:
      69 │         await this.N.error(12558null"Try saving or reverting the editors with unsaved changes first and then try again."));
         ╵                                  ^

X [ERROR] Syntax error "u"

    out-build/vs/workbench/services/files/electron-sandbox/elevatedFileService.js:43:32:
      43 │             message: $l ? 12914null"You are about to save '{0}' as admin.", this.e.getUriLabel(12915null"You are about to save '{0}' as super user.", this.e.getUriLabel(resource)),
         ╵                                 ^

X [ERROR] Syntax error "u"

    out-build/vs/workbench/services/workingCopy/electron-sandbox/workingCopyBackupService.js:35:89:
      35 │         this.B(this.c.onWillShutdown(event => event.join(this.joinBackups(), { id: 13617null"Backup working copies") })));
         ╵                                                                                          ^

X [ERROR] Syntax error "n"

    out-build/vs/workbench/services/workspaces/electron-sandbox/workspaceEditingService.js:141:42:
      141 │         const stopped = await this.W.13643null"Opening a multi-root workspace."));
          ╵                                           ^

X [ERROR] Expected ")" but found "\"Whether the platform has the WSL feature installed\""

    out-build/vs/workbench/contrib/remote/electron-sandbox/remote.contribution.js:155:86:
      155 │         const hasWSLFeatureContext = new $nn(contextKeyId, !!defaultValue, nls9298null"Whether the platform has the WSL feature installed"));
          │                                                                                       ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
          ╵                                                                                       )

X [ERROR] Syntax error "u"

    out-build/vs/workbench/electron-sandbox/desktop.main.js:134:124:
      134 │         this.B(workbench.onWillShutdown(event => event.join(storageService.close(), { id: 'join.closeStorage', label: 12353null"Saving UI state") })));
          ╵                                                                                                                             ^

[13:30:40] 'vscode-win32-x64-min' errored after 59 min
[13:30:40] Error: Build failed with 1 error:
out-build/vs/code/node/cliProcessMain.js:113:75: ERROR: Syntax error "l"
    at failureErrorWithLog (D:\LearningMaterials\智能软件工程组\华为@workspace@codechat\意图预测\iseg-ide-sub1\vscode-restoreAPIs\build\node_modules\esbuild\lib\main.js:1472:15)
    at D:\LearningMaterials\智能软件工程组\华为@workspace@codechat\意图预测\iseg-ide-sub1\vscode-restoreAPIs\build\node_modules\esbuild\lib\main.js:945:25
    at runOnEndCallbacks (D:\LearningMaterials\智能软件工程组\华为@workspace@codechat\意图预测\iseg-ide-sub1\vscode-restoreAPIs\build\node_modules\esbuild\lib\main.js:1315:45)
    at buildResponseToResult (D:\LearningMaterials\智能软件工程组\华为@workspace@codechat\意图预测\iseg-ide-sub1\vscode-restoreAPIs\build\node_modules\esbuild\lib\main.js:943:7)
    at D:\LearningMaterials\智能软件工程组\华为@workspace@codechat\意图预测\iseg-ide-sub1\vscode-restoreAPIs\build\node_modules\esbuild\lib\main.js:970:16
    at responseCallbacks.<computed> (D:\LearningMaterials\智能软件工程组\华为@workspace@codechat\意图预测\iseg-ide-sub1\vscode-restoreAPIs\build\node_modules\esbuild\lib\main.js:622:9)
    at handleIncomingPacket (D:\LearningMaterials\智能软件工程组\华为@workspace@codechat\意图预测\iseg-ide-sub1\vscode-restoreAPIs\build\node_modules\esbuild\lib\main.js:677:12)
    at Socket.readFromStdout (D:\LearningMaterials\智能软件工程组\华为@workspace@codechat\意图预测\iseg-ide-sub1\vscode-restoreAPIs\build\node_modules\esbuild\lib\main.js:600:7)
    at Socket.emit (node:events:519:28)
    at Socket.emit (node:domain:551:15)
```

