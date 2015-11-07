---
layout: post
title: "模型动画查看器"
date: 2014-10-30 23:48:44 +0800
comments: true
categories: [Unity]
---

美术制作的3D模型动画文件，没有很好的预览手段，往往需要手动创建Animator,然后再拖对应的动画文件到State中，在模型动画数量增多的时候，手动处理这些问题往往是耗时低效的。所以就有了制作模型动画查看器的需求。  

基本的思路为读取模型文件，创建Animation Controller, 将动画文件添加到Animation Controller的Layer，并建立不同状态间的Transition. 代码如下:

``` C#
using UnityEngine;
using System.Collections;
using UnityEditor;
using UnityEditorInternal;
using System.IO;
using System.Collections.Generic;

public class AnimationViewer : Editor
{
	private static string ModelPath = Application.dataPath + "/Model";
	private static string animationControllerPath = "Assets/AnimationController";
	private static string PrefabPath = "Assets/Resources/Prefabs";
	private static string FileName = "ModelNames"; 
	private static StreamWriter writer;
	
	static void DoCreateAnimationAssets ()
	{
		//获得模型动画所在文件夹
		DirectoryInfo raw = new DirectoryInfo (ModelPath);		
		string appendString = "";

		//遍历模型动画文件夹
		foreach (DirectoryInfo dictory in raw.GetDirectories()) 
		{
			appendString += ":" + dictory.Name;
			State lastState = null;

			//生成AnimationController
			AnimatorController animatorController = AnimatorController.CreateAnimatorControllerAtPath (animationControllerPath + "/" + dictory.Name + ".controller");
			//得到AnimationController的Layer， 默认layer为base
			AnimatorControllerLayer layer = animatorController.GetLayer (0);
			List<State> stateList = new List<State>();
			foreach (FileInfo file in dictory.GetFiles()) {
				//动画名称遵循"模型名@动画名"的规则，并除去.meta文件
				if (file.Name.Contains ("@") && !file.Name.Contains (".meta")) {
					//把动画文件保存在我们创建的AnimationController中，并与上一个动画状态做Transition.
					string fbxPath = "Assets/Model/" + dictory.Name + "/" + file.Name;
					lastState = AddStateTransition (fbxPath, layer, lastState);
					stateList.Add(lastState);
				}
			}

			UnityEditorInternal.StateMachine sm = layer.stateMachine;
			//首位详解，形成一个循环播放的效果
			sm.AddTransition(stateList[stateList.Count -1], stateList[0]);

			//保存为Prefab
			BuildPrefab (dictory, animatorController);
		}

		AppendString (appendString);
	}

	static void AppendString (string appendString)
	{
		//记录所有的模型名称
		File.WriteAllText (Application.dataPath + "/Resources/" + FileName + ".txt", appendString);
	}

	private static State AddStateTransition (string path, AnimatorControllerLayer layer, State lastState)
	{
		UnityEditorInternal.StateMachine sm = layer.stateMachine;
		//根据动画文件读取它的AnimationClip对象
		AnimationClip newClip = AssetDatabase.LoadAssetAtPath (path, typeof(AnimationClip)) as AnimationClip;
		AnimationUtility.SetAnimationType (newClip, ModelImporterAnimationType.Generic);
		//取出动画名子 添加到state里面
		State state = sm.AddState (newClip.name);
		state.SetAnimationClip (newClip, layer);
		//把state添加在layer里面
		if (lastState ！= null) 
		{
			sm.AddTransition (lastState, state);
		}

		return state;
	}

	private static void BuildPrefab (DirectoryInfo dictory, AnimatorController animatorCountorller)
	{
		string modelPath = "Assets/Model/" + dictory.Name + "/" + dictory.Name + ".FBX";

		GameObject go = AssetDatabase.LoadAssetAtPath (modelPath, typeof(GameObject)) as GameObject;
		go.name = dictory.Name;

		Animator animator = go.GetComponent<Animator> ();
		animator.runtimeAnimatorController = animatorCountorller;

		PrefabUtility.CreatePrefab (PrefabPath + "/" + go.name + ".prefab", go);
	}
}
```
