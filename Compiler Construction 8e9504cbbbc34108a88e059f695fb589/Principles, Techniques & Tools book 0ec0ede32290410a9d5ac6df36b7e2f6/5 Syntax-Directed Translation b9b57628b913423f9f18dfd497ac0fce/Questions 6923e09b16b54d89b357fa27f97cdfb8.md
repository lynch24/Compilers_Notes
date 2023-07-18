# Questions

- What is a syntax-directed definition?
    
    A context free grammar together with rules and attributes
    
- What is a synthesised attribute?
    
    An attribute at node N defined only in terms of the attribute values at the children of N and itself
    
- What is an inherited attribute?
    
    An attribute at node N that is defined only in terms of attribute values at N’s parent, N itself and N’s siblings
    
- What is a S-attributed SDD?
    
    An SDD that involves only synthesised attributes
    
- What is an annotated parse tree?
    
    A parse tree that shows the values of its attributes
    
- How do we evaluate synthesised attributes?
    
    In any bottom-up order, such as a postorder traversal