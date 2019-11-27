<h4>・11/21</h4>

Font Asset のマテリアルをインスペクタで表示し、コンテキストメニューの「Create Material Preset」でマテリアルを作成した際に、 Assets 直下など変な場所に変な名前で作成されてしまい、 Text のインスペクタの「Material Preset」のドロップダウンに候補として出てこないことがあった

`Library/PackageCache/com.unity.textmeshpro@1.4.1/Scripts/Editor/TMPro_ContextMenus.cs` を見ると

```csharp
            string assetPath = AssetDatabase.GetAssetPath(source_Mat).Split('.')[0];

            // (略)

            AssetDatabase.CreateAsset(duplicate, AssetDatabase.GenerateUniqueAssetPath(assetPath + ".mat"));
```

という実装なので、 **Font Asset のアセットパスに「.」 (ドット) が含まれている** と変なところに変な名前で作られてしまうという訳

自分は名前空間形式フォルダを多用するスタイルなので、もうこれは諦める。。

場所と名前が思ったのと違うだけで、アセットの作成自体に問題はない

また `Library/PackageCache/com.unity.textmeshpro@1.4.1/Scripts/Editor/TMP_EditorUtility.cs` を見ると、

```csharp
            // Get materials matching the search pattern.
            string searchPattern = "t:Material" + " " + fontAsset.name.Split(new char[] { ' ' })[0];
            string[] materialAssetGUIDs = AssetDatabase.FindAssets(searchPattern);

            for (int i = 0; i < materialAssetGUIDs.Length; i++)
            {
                string materialPath = AssetDatabase.GUIDToAssetPath(materialAssetGUIDs[i]);
                Material targetMaterial = AssetDatabase.LoadAssetAtPath<Material>(materialPath);

                if (targetMaterial.HasProperty(ShaderUtilities.ID_MainTex) && targetMaterial.GetTexture(ShaderUtilities.ID_MainTex) != null && mat.GetTexture(ShaderUtilities.ID_MainTex) != null && targetMaterial.GetTexture(ShaderUtilities.ID_MainTex).GetInstanceID() == mat.GetTexture(ShaderUtilities.ID_MainTex).GetInstanceID())
                {
                    if (!refs.Contains(targetMaterial))
                        refs.Add(targetMaterial);
                }
                else
                {
                    // TODO: Find a more efficient method to unload resources.
                    //Resources.UnloadAsset(targetMaterial.GetTexture(ShaderUtilities.ID_MainTex));
                }
            }
```

という実装なので、「Material Preset」のドロップダウンに候補として表示するには **ファイル名のフォント名部分** (※正確には最初のスペースまでの部分) が一致している必要がある

まぁ変な名前で作られたらそのままにせず、ちゃんとTMPの想定しているフォーマットに則った名前に変えておけばいいだけのこと

またちゃんとフォントテクスチャのIDも見てるので、仮に名前だけ同じにしても別のフォントアセットのドロップダウンに出てくることはない
