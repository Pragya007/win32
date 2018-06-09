---
Description: The Auto Claims sample addresses a hypothetical scenario for an insurance assessor.
ms.assetid: bec4333a-62ca-4254-a39b-04bc2c556992
title: Auto Claims Form Sample
ms.technology: desktop
ms.prod: windows
ms.author: windowssdkdev
ms.topic: article
ms.date: 05/31/2018
---

# Auto Claims Form Sample

The Auto Claims sample addresses a hypothetical scenario for an insurance assessor. The assessor's work requires him or her to visit with clients at their home or business and to enter their claim information into a form. To increase the assessor's productivity, his IT department develops a tablet application that enables him or her to quickly and accurately enter claim information through two ink controls: [InkEdit](https://www.bing.com/search?q=InkEdit) and [InkPicture](https://www.bing.com/search?q=InkPicture) controls.

In this sample, a [InkEdit](https://www.bing.com/search?q=InkEdit) control is used for each text input field. A user enters the relevant information about an insurance policy and vehicle into these fields with a pen. The [InkPicture](https://www.bing.com/search?q=InkPicture) control is used to add ink over an automobile image to highlight damaged areas of the automobile. The Auto Claims sample is available for C\# and Microsoft Visual Basic .NET. This topic describes the Visual Basic .NET.

The AutoClaims Class is defined as a subclass of [System.Windows.Forms.Form](https://www.bing.com/search?q=System.Windows.Forms.Form) , and a nested class is defined for creating and managing layers of ink for different types of damage. Four event handlers are defined to perform the following tasks:

-   Initializing the form and ink layers.
-   Redrawing the [InkPicture](https://www.bing.com/search?q=InkPicture) control.
-   Selecting an ink layer through the list box.
-   Changing the visibility of an ink layer.

> [!Note]  
> Versions of this sample are available in C\# and Visual Basic .NET. The version discussed in this section is Visual Basic .NET. The concepts are the same between versions.

 

## Defining the Form and Ink Layers

You need to import the [Microsoft.Ink](https://www.bing.com/search?q=Microsoft.Ink) namespace:


```VB
Imports Microsoft.Ink
```




```VB
// The Ink namespace, which contains the Tablet PC Platform API
using Microsoft.Ink;
```



Next, in the AutoClaims Class, a nested `InkLayer` class is defined and an array of four `InkLayer` objects is declared. (InkLayer contains an [Microsoft.Ink.Ink](https://www.bing.com/search?q=Microsoft.Ink.Ink) object for storing ink, and [System.Drawing.Color](https://www.bing.com/search?q=System.Drawing.Color) and **Boolean** values for storing the color and hidden state of the layer.) A fifth Ink object is declared to handle ink for the [InkPicture](https://www.bing.com/search?q=InkPicture) when all of the ink layers are hidden.


```VB
' Declare the array of ink layers used the vehicle illustration.
Dim inkLayers(3) As InkLayer

' Declare an empty ink object (used when we don't want to draw
' any ink).
Dim emptyInk As Ink

' Declare a value to hold the index of selected ink
Dim selectedIndex As Integer

' Declare a value to hold whether the selected ink is hidden
Dim selectedHidden As Boolean 
```




```VB
// Declare the array of ink layers used the vehicle illustration.
InkLayer[] inkLayers;

// Declare an empty ink object (used when we don't want to draw
// any ink).
Ink emptyInk;

// Declare a value to hold the index of selected ink
int selectedIndex = -1;

// Declare a value to hold whether the selected ink is hidden
bool selectedHidden = false;
```



Each layer has its own [Ink](https://www.bing.com/search?q=Ink) object. There are four discrete areas of interest in the claim form (body, windows, tires, and headlights), so four InkLayer objects are used. A user can view any combination of layers at once.

## Initializing the Form and Ink Layers

The `Load` event handler initializes the [Ink](https://www.bing.com/search?q=Ink) object and the four `InkLayer` objects.


```VB
' Initialize the empty ink
emptyInk = New Ink()

' Initialize the four different layers of ink on the vehicle diagram:  
' vehicle body, windows, tires, and headlights.
inkLayers(0) = New InkLayer(New Ink(), Color.Red, False)
inkLayers(1) = New InkLayer(New Ink(), Color.Violet, False)
inkLayers(2) = New InkLayer(New Ink(), Color.LightGreen, False)
inkLayers(3) = New InkLayer(New Ink(), Color.Aqua, False)
```




```VB
// Initialize the empty ink
emptyInk = new Ink();

// Initialize the four different layers of ink on the vehicle diagram:  
// vehicle body, windows, tires, and headlights.
inkLayers = new InkLayer[4];
inkLayers[0] = new InkLayer(new Ink(), Color.Red, false);
inkLayers[1] = new InkLayer(new Ink(), Color.Violet, false);
inkLayers[2] = new InkLayer(new Ink(), Color.LightGreen, false);
inkLayers[3] = new InkLayer(new Ink(), Color.Aqua, false);
```



Then, select the first entry (Body) in the list box.


```VB
' By default, select the first ink layer
lstAnnotationLayer.SelectedIndex = 0
```




```VB
// By default, select the first ink layer
lstAnnotationLayer.SelectedIndex = 0;
```



Finally, set the ink color for the [InkPicture](https://www.bing.com/search?q=InkPicture) control to the currently selected list box entry.


```VB
inkPictVehicle.DefaultDrawingAttributes.Color =
      inkLayers(lstAnnotationLayer.SelectedIndex).ActiveColor
```




```VB
inkPictVehicle.DefaultDrawingAttributes.Color = inkLayers[lstAnnotationLayer.SelectedIndex].ActiveColor;
```



## Redrawing the InkPicture Control

In the [InkPicture](https://www.bing.com/search?q=InkPicture) Control's inherited [Paint](https://www.bing.com/search?q=Paint) event handler, the ink layers are checked to determine which are hidden. If a layer is not hidden, the event procedure displays it by using the [Renderer](https://www.bing.com/search?q=Renderer) property's [Draw](https://www.bing.com/search?q=Draw) method. If you look in the Object Browser, you'll see that the Microsoft.Ink.InkPicture.Renderer property is defined as a [Microsoft.Ink.Renderer](https://www.bing.com/search?q=Microsoft.Ink.Renderer) object:


```VB
Private Sub inkPictVehicle_Paint(ByVal sender As System.Object, ByVal e As System.Windows.Forms.PaintEventArgs) Handles inkPictVehicle.Paint
    Dim layer As InkLayer

    ' Cycle through each ink layer.  If it is visible, paint it.
    ' Note that it is necessary to customize the paint
    ' behavior, since we want to hide/show different ink layers.
    For Each layer In inkLayers
        If (Not layer.Hidden) Then
            inkPictVehicle.Renderer.Draw(e.Graphics, layer.ActiveInk.Strokes)
        End If
    Next
End Sub
```




```VB
private void inkPictVehicle_Paint(object sender, System.Windows.Forms.PaintEventArgs e)
{
    // Cycle through each ink layer.  If it is visible, paint it.
    // Note that it is necessary to customize the paint
    // behavior, since we want to hide/show different ink layers.
    foreach (InkLayer layer in inkLayers)
    {
        if (!layer.Hidden)
        {
             inkPictVehicle.Renderer.Draw(e.Graphics,layer.ActiveInk.Strokes);
        }
    }          
}
```



## Selecting an Ink Layer through the List Box

When the user selects an item in the list box, the [SelectedIndexChanged](https://www.bing.com/search?q=SelectedIndexChanged) event handler first checks that the selection has changed and that the [InkPicture](https://www.bing.com/search?q=InkPicture) control is not currently collecting ink. It then sets the ink color of the InkPicture control to the appropriate color for the selected ink layer. Also, it updates the Hide Layer check box to reflect the selected ink layer's hidden status. Finally, the InkPicture control's inherited [Refresh](https://www.bing.com/search?q=Refresh) method is used to display only the desired layers within the control.


```VB
Private Sub chHideLayer_CheckedChanged(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles chHideLayer.CheckedChanged

    ' Provided that the new checked hidden value is different than
    ' the previous value...
    If (Not (chHideLayer.Checked = selectedHidden)) Then
        If (Not (inkPictVehicle.CollectingInk)) Then

            ' Update the array indicating the visibility of each ink layer
            inkLayers(lstAnnotationLayer.SelectedIndex).Hidden = chHideLayer.Checked

            ' Set the active ink object to the selected ink
            ' Note that if the current layer is not visible, empty
            ' ink is used to prevent flicker.
            inkPictVehicle.InkEnabled = False
            If (chHideLayer.Checked) Then
                inkPictVehicle.Ink = emptyInk
            Else
                inkPictVehicle.Ink = inkLayers(lstAnnotationLayer.SelectedIndex).ActiveInk
            End If

            ' Update the previous checkbox value to the current
            selectedHidden = chHideLayer.Checked

            ' If the layer is marked hidden, disable ink collection
            inkPictVehicle.InkEnabled = Not chHideLayer.Checked

            Me.Refresh()
       Else
            ' If ink collection is enabled, the active ink cannot be changed
            ' and it is necessary to restore the checkbox to its previous value.
            chHideLayer.Checked = selectedHidden
            MessageBox.Show("Cannot change visiblity while collecting ink.")
       End If
   End If
End Sub
```




```VB
private void lstAnnotationLayer_SelectedIndexChanged(object sender, System.EventArgs e)
{
    // Provided that the new selected index value is different than
    // the previous value...
    if (lstAnnotationLayer.SelectedIndex != selectedIndex) 
    {
        if (!inkPictVehicle.CollectingInk)
        {
            // Set the ink and visiblity of the current ink layer
            inkPictVehicle.DefaultDrawingAttributes.Color = inkLayers[lstAnnotationLayer.SelectedIndex].ActiveColor;
            chHideLayer.Checked = inkLayers[lstAnnotationLayer.SelectedIndex].Hidden;

            // Set the active ink object to the selected ink
            // Note that if the current layer is not visible, empty
            // ink is used to prevent flicker.
            inkPictVehicle.InkEnabled = false;
            inkPictVehicle.Ink = chHideLayer.Checked?emptyInk:inkLayers[lstAnnotationLayer.SelectedIndex].ActiveInk;
            inkPictVehicle.InkEnabled = !chHideLayer.Checked;
    
            selectedIndex = lstAnnotationLayer.SelectedIndex;

            this.Refresh();
        }
        else 
        {
            // If ink collection is enabled, the active ink cannot be changed
            // and it is necessary to restore the selection to its previous value.
            lstAnnotationLayer.SelectedIndex = selectedIndex;
            MessageBox.Show("Cannot change active ink while collecting ink.");
        }
    }
}
```



## Changing the Visibility of an Ink Layer

The `CheckedChanged` event handler first checks that the selection has changed and that the [InkPicture](https://www.bing.com/search?q=InkPicture) control is not currently collecting ink. It then updates the selected ink layer's hidden status, sets the InkPicture control's InkEnabled to **FALSE**, .

Next, the InkPicture control's InkEnabled property is set to **FALSE** before updating its Ink property.

Finally, the [InkPicture](https://www.bing.com/search?q=InkPicture) control is either enabled or disabled for the particular vehicle part based on whether the Hide Layer check box is selected, and the InkPicture control's [Refresh](https://www.bing.com/search?q=Refresh) method is used to display only the desired layers within the control.


```VB
Private Sub chHideLayer_CheckedChanged(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles chHideLayer.CheckedChanged

    ' Provided that the new checked hidden value is different than
    ' the previous value...
    If (Not (chHideLayer.Checked = selectedHidden)) Then
        If (Not (inkPictVehicle.CollectingInk)) Then

            ' Update the array indicating the visibility of each ink layer
            inkLayers(lstAnnotationLayer.SelectedIndex).Hidden = chHideLayer.Checked

            ' Set the active ink object to the selected ink
            ' Note that if the current layer is not visible, empty
            ' ink is used to prevent flicker.
            inkPictVehicle.InkEnabled = False
            If (chHideLayer.Checked) Then
                inkPictVehicle.Ink = emptyInk
            Else
                inkPictVehicle.Ink = inkLayers(lstAnnotationLayer.SelectedIndex).ActiveInk
            End If

            ' Update the previous checkbox value to the current
            selectedHidden = chHideLayer.Checked

            ' If the layer is marked hidden, disable ink collection
            inkPictVehicle.InkEnabled = Not chHideLayer.Checked

            Me.Refresh()
        Else
            ' If ink collection is enabled, the active ink cannot be changed
            ' and it is necessary to restore the checkbox to its previous value.
            chHideLayer.Checked = selectedHidden
            MessageBox.Show("Cannot change visiblity while collecting ink.")
        End If
    End If
End Sub
```




```VB
private void chHideLayer_CheckedChanged(object sender, System.EventArgs e)
{
    // Provided that the new checked hidden value is different than
    // the previous value...
    if (chHideLayer.Checked != selectedHidden) 
    {
        if (!inkPictVehicle.CollectingInk)
        {
            // Update the array indicating the visibility of each ink layer
            inkLayers[lstAnnotationLayer.SelectedIndex].Hidden = chHideLayer.Checked;

            // Set the active ink object to the selected ink
            // Note that if the current layer is not visible, empty
            // ink is used to prevent flicker.
            inkPictVehicle.InkEnabled = false;
            inkPictVehicle.Ink = chHideLayer.Checked?emptyInk:inkLayers[lstAnnotationLayer.SelectedIndex].ActiveInk;
 
            // If the layer is marked hidden, disable ink collections
            inkPictVehicle.InkEnabled = !chHideLayer.Checked;

            // Update the previous checkbox value to reflect the current
            selectedHidden = chHideLayer.Checked;

            this.Refresh();
        }
        else 
        {
            // If ink collection is enabled, the active ink cannot be changed
            // and it is necessary to restore the checkbox to its previous value.
            chHideLayer.Checked = selectedHidden;
            MessageBox.Show("Cannot change visiblity while collecting ink.");
        }
    }
}
```



## Closing the Form

In the Windows Form Designer generated code, the [InkEdit](https://www.bing.com/search?q=InkEdit) and [InkPicture](https://www.bing.com/search?q=InkPicture) controls are added to the form's component list when the form is initialized. When the form closes, the InkEdit and InkPicture controls are disposed, as well as the other components of the form, by the form's [Dispose](https://www.bing.com/search?q=Dispose) method. The form's Dispose method also disposes the [Ink](https://www.bing.com/search?q=Ink) objects that are created for the form.

## Related topics

<dl> <dt>

[Microsoft.Ink.Ink](https://www.bing.com/search?q=Microsoft.Ink.Ink)
</dt> <dt>

[InkPicture Control](inkpicture-control.md)
</dt> <dt>

[InkEdit Control](inkedit-control.md)
</dt> </dl>

 

 


