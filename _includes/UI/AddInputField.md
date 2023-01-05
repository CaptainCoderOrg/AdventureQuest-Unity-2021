<details markdown="block">
<summary>
    <h3 style="display:inline">Add an InputField (Click to Expand)</h3>
</summary>

1. Right click in the `Hierarchy`
2. Select `UI` > `Input Field - TextMeshPro`
3. Rename the component (it is bad practice to leave the default name.)

![Add Button](/pages/imgs/UI/05-AddInputField.png)

### Children of InputField

An `InputField` comes with a child, `TextArea`, which has 2 
additional children: `Placeholder` and `Text`. 

![Grandchildren Components](/pages/imgs/UI/06-InputTextFields.png)

Both the `Placeholder` and `Text` children are `Text` **components**.

There is **RARELY** a reason to modify the `TextArea` child.
{: .warning }

* `Placeholder` will be displayed if `InputField` is blank. This is typically used to give instructions to the user.
* `Text` is automatically updated to match whatever text the user has typed in the `InputField`.

Typically, the styling of these two components are identical (with the
`Placeholder` being a slightly lighter color). If you adjust the styling on one,
it does not automatically adjust the styling on the other. However, if you
select both (hold shift and click) at the same time, you can modify both in the
`Inspector` at the same time. The image below demonstrates how to do this. 
{: .tip }

![Adjust Multiple Elements](/pages/imgs/UI/07-ChangeMultipleTextFields.gif)


</details>