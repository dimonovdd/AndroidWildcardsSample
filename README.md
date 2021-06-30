This sample demonstrates two problems related to the csproj file on Android. [Problem link](https://developercommunity.visualstudio.com/t/Problem-with-wildcard-in-Android-csproj/1464993)

## [First (ProjectReference with Wildcard)](https://github.com/dimonovdd/ProjectReferenceWildcardsSample/blob/51d43422e79354acf82db8e09c84377f091579f7/Sample.Android/Sample.Android.csproj#L51)

```xml
<ProjectReference Include="..\Plugins\*\*.csproj" />
```
This line will be expanded into what you see below when you open the project in VS4Mac. This works correctly in iOS or .Net Standart projects.

```xml
<ProjectReference Include="..\Plugins\TestPlugin2\TestPlugin2.csproj">
    <Targets></Targets>
    <OutputItemType></OutputItemType>
    <ReferenceSourceTarget>ProjectReference</ReferenceSourceTarget>
</ProjectReference>
<ProjectReference Include="..\Plugins\TestPlugin1\TestPlugin1.csproj">
    <Targets></Targets>
    <OutputItemType></OutputItemType>
    <ReferenceSourceTarget>ProjectReference</ReferenceSourceTarget>
</ProjectReference>
```

## [Second (Compile with Wildcard)](https://github.com/dimonovdd/ProjectReferenceWildcardsSample/blob/51d43422e79354acf82db8e09c84377f091579f7/Sample.Android/Sample.Android.csproj#L51)

```xml
<ItemGroup> 
    <Compile Include="**\*.cs" Exclude="obj\**" /> 
</ItemGroup> 
```

A project will not be built the first time with this ItemGroup (The next ItemGroup is an ugly hack to solve).
`Resources\Resource.designer.cs` will be generated but it will not be included in the project at the first compilation. this problem
It doesn't depend on a IDE it doesn't work correctly with MSBuild on Terminal

```xml
<ItemGroup>
    <Compile Include="**\*.cs" Exclude="$(AndroidResgenFile);obj\**" />
    <Compile Include="$(AndroidResgenFile)" />
</ItemGroup> 
```

This will work correctly if you add this line to csproj ([@mattleibow](https://github.com/mattleibow) thank you for this information):
```xml
<AndroidUseIntermediateDesignerFile>true</AndroidUseIntermediateDesignerFile>
```


