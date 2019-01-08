---
title: AllowedMockableTypes Class
page_title: AllowedMockableTypes Class | JustMock Documentation
description: Documentation page about AllowedMockableTypes Class.
previous_url: /t_telerik_justmock_setup_allowedmockabletypes.html
slug: t_telerik_justmock_setup_allowedmockabletypes
published: True
fullPath: api/n_telerik_justmock_setup/t_telerik_justmock_setup_allowedmockabletypes/t_telerik_justmock_setup_allowedmockabletypes
category: "api"
---

# AllowedMockableTypes Class



Contains a list of types that are exempt from sanity checks when mocking.


 **Namespace:**  [Telerik.JustMock.Setup](n_telerik_justmock_setup) <br> **Assembly:** Telerik.JustMock(in Telerik.JustMock.dll)
## Syntax


<div id="syntaxCodeBlocks" class="code"><span codeLanguage="CSharp"><table><tr><th>C#</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">public</span> <span class="keyword">static</span> <span class="keyword">class</span> <span class="identifier">AllowedMockableTypes</span></pre></td></tr></table></span><span codeLanguage="VisualBasicDeclaration"><table><tr><th>Visual Basic</th></tr><tr><td><pre xml:space="preserve"><span class="keyword">Public</span> <span class="keyword">NotInheritable</span> <span class="keyword">Class</span> <span class="identifier">AllowedMockableTypes</span></pre></td></tr></table></span></div>


## Remarks


It might not be safe to mock some types, but sometimes other types might be safe but come out as false positives in the sanity checks. Add these types to the list to try to mock them anyway. Mind that mocking certain types will not be possible, even if they're added to this list. Also mind that

## Inheritance Hierarchy


* [System.Object](e5kfa45b)

    * Telerik.JustMock.Setup.AllowedMockableTypes


## See Also



 [AllowedMockableTypes Members](allmembers_t_telerik_justmock_setup_allowedmockabletypes) 

 [Telerik.JustMock.Setup Namespace](n_telerik_justmock_setup) 



