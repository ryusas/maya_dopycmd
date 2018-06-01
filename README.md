# maya_dopycmd
A tiny Maya plug-in for executing any python callable object as command that can be undo and redo.

References:
    https://medium.com/@k_serguei/maya-python-api-2-0-and-the-undo-stack-80b84de70551

The above is an API story, but I have generalized it to be usable with python callable object.

Usage:

    import maya.cmds as cmds
    cmds.loadPlugin('dopycmd')

    def docmd(do, undo):
        cmds.dopycmd(hex(id(do)), hex(id(undo)))

    def doapi(mod):
        #cmds.dopycmd(hex(id(mod.doIt)), hex(id(mod.undoIt)))  # cause crash
        docmd(lambda: mod.doIt(), lambda: mod.undoIt())

    val = cmds.jointDisplayScale(q=True)
    docmd(lambda: cmds.jointDisplayScale(10), lambda: cmds.jointDisplayScale(val))

    import maya.api.OpenMaya as api
    mod = api.MDagModifier()
    mod.createNode('transform')
    doapi(mod)
    del mod

    cmds.undo()
    cmds.undo()
